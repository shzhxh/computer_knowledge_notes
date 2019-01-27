#### archLinux安装过程分析

本文是对archlinux安装过程原理的分析，而不是安装引导，具体的安装步骤请参照wiki.archlinux.org。

- 我尝试过如下安装方法：
  - 镜像引导，在qemu上安装
  - 镜像引导，在真实主机上安装
  - 网络引导，在真实主机上安装

1. 不管是用什么方法安装，第一步是要安装一个可运行的系统。
   - 这个系统是用来安装系统的系统，而不是要安装的系统本身。虽然没有分析过它，但我想它应该包括了内核、shell和一些必须的工具。
   - 从理论上来讲，这个系统也不必是linux，虽然它目前是linux。它只要能把archlinux安装到本机即可。
2. 第二步是要为archlinux分配空间
   - 只有根目录的磁盘分区是必须要分配的
   - 建议为私人文件分配独立分区，这样系统奔溃的时候方便恢复私人数据
   - 如果内存较小，应分配交换分区，这样可以充分利用linux内存管理的优势
   - 如果系统支持，尽量使用uefi+gpt方式，这样可以突破mbr+bios方式的局限，其实是更简单更自由了
   - 如果是固态+机械两块硬盘，应将读写频繁的分区放在机械硬盘上，以延长固态硬盘的寿命
3. 第三步是把archlinux的基本系统安装到上一步分配的空间中
4. 第四步切换用户到上步安装的archlinux系统中，进行必要的设置
   - 时区设置，个人认为不是必须的
   - 时钟设置
   - 语言设置
   - 主机名设置
   - mkinitcpio不知道是起什么作用，据说是为了取代老旧的klibc
   - 设置用户密码
   - 安装引导器（如果是uefi引导，应把grub安装在uefi的引导分区）
5. 第五步，上一步已经成功安装了一个可引导的archlinux了，本步是做一些善后工作。
   - 返回安装archlinux之前的系统
   - 取消对archlinux系统的挂载
   - 关机
   - 如果是网络安装，要修改启动顺序，否则会再次启动安装程序



#### 在qemu上安装archLinux

1. qemu的网络采用tun/tap方式，这是最好连网方式
```
	ls -l /dev/net/tun	//测试是否内建于内核中，否则测试是否在模块中。注：2.6.x之后的版本均已内建于内核中
	lsmod | grep tun	//测试模块是否包含tun/tap，否则需要手动加载到内核模块里
	/*如下代码手动加载内核模块*/
	sudo apt install linux-source	//下载内核源代码
	sudo tar xf linux-source	//解压源码
	sudo cp /boot/config ./		//将当前内核的配置文件复制到当前文件夹
	sudo make menuconfig		//修改配置文件
	sudo make modules
	cp drivers/net/tun.ko /lib/modules/kernel/drivers/net	//将tun.ko复制到内核模块的目录里
	depmod		//重建模块依赖关系
	modprobe tun	//加载模块
	/*如下代码配置网络*/
	sudo apt install bridge-utils uml-utilities	//安装所需软件包
	ifconfig eth0 down	//关闭eth0接口。大部分无线网卡不支持tun/tap模式，只有以太网卡支持tun/tap模式，注意！
	brctl addbr br0		//添加虚拟网桥br0
	brctl addif br0 eth0	//在br0中添加接口eth0
	brctl stp br0 off	//只有一个网桥，所以关闭生成树协议
	brctl setfd br0 1	//设置br0的转发延迟
	brctl sethello br0 1	//设置br0的hello时间
	ifconfig br0 0.0.0.0 promisc up		//打开br0接口
	ifconfig eth0 0.0.0.0 promisc up	//打开eth0接口
	dhclient br0		//从dhcp服务器获得br0的IP地址
	brctl show br0		//查看虚拟网桥列表
	brctl showstp br0	//查看br0的各接口信息
	tunctl -t tap0 -u root 	//创建tap0接口，只允许root用户访问
	brctl addif br0 tap0	//在虚拟网桥增加一个tap接口
	ifconfig tap0 0.0.0.0 promisc up	//打开tap0接口
	brctl showstp br0	//显示br0的各个接口
```

2. 安装archLinux
```
	qemu-img create -f raw image_file 8G	/* to create hard disk images*/
	qemu-system-x86_64 -m 1G -cdrom iso_image -boot order=d -net nic -net tap,ifname=tap0,script=no,downscript=no -drive file=disk_image,format=raw	/* installing the OS*/
	
	/*Pre-installation*/
	timedatectl set-ntp true	//ensure the system clock is accurate
	fdisk /dev/sda		//partition the disk, /dev/sda1 511M, /dev/sda2 7.5G
	mkfs.ext4 /dev/sda2	//format the partitions
	mkswap	/dev/sda1	//set the swap space
	mount /dev/sda2 /mnt
	pacstrap /mnt base	//install the base packages
	genfstab -U /mnt >> /mnt/etc/fstab	//generate a fstab file
	arch-chroot /mnt	//change root into the new system
	ln -sf /usr/share/zoneinfo/Region/City /etc/localtime	//set the time zone
	hwclock	--systohc	//to generate /etc/adjtime
	/etc/locale.gen		//uncomment en_US.UTF-8 UTF-8
	locale-gen		//generate the changes in /etc/locale.gen
	/etc/locale.conf	//set the LANG variable with en_US.UTF-8
	/etc/hostname		//create the hostname
	/etc/hosts		//add a matching entry to hosts
	mkinitcpio -p linux	//mkinitcpio was run on installation of the linux package with pacstrap
	passwd			//set the root password
	pacman -S grub		//install grub
	grub-install --target=i386-pc /dev/sda
	grub-mkconfig -o /boot/grub/grub.cfg
	exit			//exit the chroot environment
	umount -R /mnt		//unmount all the partitions
	shutdown now
```

3. 启动archlinux
```
	qemu-system-x86_64 -m 1G -net nic -net user -drive file=disk_image,format=raw
	//也许可以试试 -netdev tap,id=tapnet,ifname=tap0,script=no -device rtl8139,netdev=tapnet
	systemctl enable dhcpcd		//开启dhcp才可以联网
	pacman -S net-tools		//这样才可以使用ifconfig等工具
```

#### U盘安装

##### 1. 制作启动盘

```bash
mkdir -p /mnt/{iso,usb}
mount -o loop archlinux-2017.04.01-x86_64.iso /mnt/iso
mount /dev/sdb /mnt/usb
cp -a /mnt/iso/* /mnt/usb
sync
umount /mnt/iso

vim /mnt/usb/loader/entries/archiso-x86_64.conf # 对于UEFI引导，需要保证标签名一致
cp /usr/lib/syslinux/modules/efi64/*.c32 /mnt/usb/arch/boot/syslinux/
extlinux --install /mnt/usb/arch/boot/syslinux
umount /mnt/usb
# 将/dev/sdb分区设置为bootable
```

##### 2. 安装

```shell
timedatectl set-ntp true	# 更新系统时间
mount /dev/sda2 /mnt		# 挂载分区
vim /etc/pacman.d/mirrorlist	# 选择镜像
pacstrap /mnt base			# 安装基本系统
genfstab -U /mnt >> /mnt/etc/fstab	# 生成fstab文件
arch-chroot /mnt			# change root
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime	# 设置时区
hwclock --systohc --utc		# 设置时间标准
vim /etc/locale.gen			# 配置本地化 en_US.UTF-8, zh_CN.UTF-8
locale-gen					# 生成locale信息
echo LANG=en_US.UTF-8 > /etc/locale.conf	# 提交本地化选项
echo 主机名 > /etc/hostname	# 设置主机名
vim /etc/hosts	# 添加如下信息，主机名与文件hostname中的一致
> 127.0.0.1	localhost.localdomain	localhost
> ::1		localhost.localdomain	localhost
> 127.0.1.1	主机名.localdomain	主机名
passwd	# 设置密码
pacman -S grub efibootmgr
parted /dev/sda # set 1 boot on，生成ESP分区
mount /dev/sda1 /boot/efi	# 挂载ESP分区
# 安装grub
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch_grub
pacman -S intel-ucode	# Intel CPU需要安装微码更新
grub-mkconfig -o /boot/grub/grub.cfg	# 生成启动项
exit	# 退出chroot
umount -R /mnt	# 卸载已挂载分区
reboot 	# 重启，进入新安装的系统
```

#### 配置

```bash
useradd -m -g 用户组 -s /bin/zsh 用户名	# 创建普通用户
passwd 用户名	# 为用户创建密码
pacman -S sudo	# 安装sudo
visudo	# 为用户添加sudo权限
> 用户名   ALL=(ALL) ALL
pacman -S xorg-server xorg-apps	# 安装显示服务
pacman -S xfce4 xfce4-goodies	# 安装桌面环境
startxfce4	# 启动桌面环境
pacman -S net-tools				# inconfig等网络工具

# 提示没有wd719x和aic94xx，安装之
git clone https://aur.archlinux.org/wd719x-firmware.git
cd w719x-firmware
makepkg -si
git clone https://aur.archlinux.org/aic94xx-firmware.git
cd aic94xx-firmware
makepkg -si

# 没有声音的解决方案，详情请在arch wiki里搜索ALSA
pacman -S alsa-utils alsa-firmware
alsamixer
vim /etc/asound.conf
> defaults.pcm.card 1
> defaults.pcm.device 0
> defaults.ctl.card 1

# 视频解码器
pacman -S gst-plugins-base gst-plugins-bad gst-plugins-good gst-plugins-ugly gst-libav
```

