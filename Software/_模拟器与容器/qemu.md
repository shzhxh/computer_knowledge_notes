#### 安装
```shell
mkdir build && cd build
../configure --target-list=riscv32-softmmu,riscv64-softmmu
sudo make install
```
> ERROR: glib-2.22 gthread-2.0 is required to compile QEMU
>  `sudo apt install libglib2.0-dev`
> ERROR: pixman >= 0.21.8 not present.
>  `sudo apt install libpixman-1-dev`
> ERROR: Cannot find Ninja
> `sudo apt install ninja-build`
> ERROR: pkg-config binary 'pkg-config' not found
> `sudo apt install pkg-config`
#### 名词解释

- 前端：qemu里运行的操作系统所能看到的设备。比如e1000网卡。
- 后端：宿主机所能看到的qemu上的设备。比如网络设备的后端，user和tap等。

#### 语法

```
qemu-system-riscv64 [options] [disk_image]
```

#### 选项

##### 标准选项

```
-accel	# 选择加速器的模型
-audiodev [driver=]driver,id=id[,prop[=value][,...]]
	# 添加一个音频后端driver，其id为id。
	# 可用的prop有：
	# id=identifier : 标识音频后端
	# timer-period=period : 设置音频子系统的计时器周期。默认10000(10ms)
-boot # 定义启动顺序
-cpu	# 选择CPU模型
-device <driver>[,prop[=value][,...]]
	# 添加设备。设备名为driver，prop=value用于设置该设备的属性。一个有效的属性取决于该设备本身。
	# 使用"-device help"获取可用的设备名。使用"-device <drive>,help"获取某个设备的可用属性。
	# 可用的prop有：
	## Controller/Bridge/Hub设备
	# usb-host:bus usb-bus
	## USB设备
	# qemu-xhci:bus PCI
	## 存储设备
	# virtio-9p-pci:bus PCI, "virtio-9p"的别名
	# virtio-blk-pic:bus PCI, 是"virtio-blk"的别名
	## 网络设备
	# e1000 : bus PCI,"e1000-82540em"的别名,desc "Intel Gigabit Ethernet"
	## 输入设备
	# isa-serial:bus ISA
	# usb-kbd:bus usb-bus
	# usb-mouse:bus usb-bus
	## 显示设备
	## 声音设备
	## Misc设备
	# ich9-intel-hda:bus PCI, desc "Intel HD Audio Controller (ich9)
	# intel-hda:bus PCI, desc "Intel HD Audio Controller (ich6)"
	# intel-iommu : bus系统，desc "Intel IOMMU (VT-d) DMA Remapping device"
	# vhost-vsock-pci : bus PCI
	## CPU设备
	## 未分类的设备
-global driver.property=value
-global driver=driver, property=property, value=value
	# 对driver的property设置一个默认的value。此选项用于为自动生成的设备设置属性，对于不是自动生成的设备应使用-device选项。
	# 对于q35设备，需要设置"-global ICH9-LPC.disable_s3=1 -global ICH9-LPC.disable_s4=1"
	# 对于i440FX设备，需要设置"-global PIIX4_PM.disable_s3=1 -global PIIX4_PM.disable_s4=1"
-k language
	# 指定键盘布局，默认为en-us。
-M, -machine [type=]name[,prop[=value][,...]]
	# 选择要模拟的机器
	# 可用的prop有：
	# accel选择加速器，支持的加速器有kvm,xen,hax,hvf,whpx或tcg。默认tcg
	# kernel_irqchip选择加速irq芯片的支持，选项有on,off或split。默认off
	# vmport
	# kvm_shadow_mem
	# dump-guest-core
	# mem-merge
	# igd-passthru
	# aes-key-wrap
	# dea-key-wrap
	# suppress-vmdesc
	# nvdimm
	# enforce-config-section
	# memory-encryption
-m megs	# 指定内存大小为megs，默认128M。
-name string1[,process=string2][,debug-threads=on|off]
	# 设置客户机的名称，string1是窗口名，string2是进程名
-smp n	# 设置CPU的个数为n，默认为1。
-soundhw	# 使能并使用声卡。
-version	# 打印版本信息
```



##### 块设备选项

```
-cdrom FILE	# 相当于 -drive file=FILE,index=2,media=cdrom
-blockdev
	# 创建一个新的块驱动器结点。
	## 对所有块驱动器都有效的选项
	## file的选项
	## raw的选项
	## qcow2的选项
	## 其它选项
-drive <option>[,option[,option[,...]]
	# 创建一个新的驱动器。相当于-blockdev和-device搭配使用的快捷写法。
	## -drive可以使用-blockdev的所有选项，另外，还可以使用如下选项：
	# file=FILE	 指定磁盘镜像
	# if=		 指定drive要连接的接口类型，可用类型有ide,scsi,sd,mtd,floppy,pflash,virtio,none
	# bus= ,unit=	 通过bus number和unit id指定连接位置
	# index=	 通过编号指定连接位置
	# media=	 指定媒介类型，disk或cdrom
	# cyls=, heads=, secs= [,trans=]	
	# snapshot=	
	# cache=	
	# aio=	
	# format=<fmt>	指定磁盘格式，这样就不会再探测磁盘格式了。
	# serial=	
	# saddr=	
	# swerror=, rerror=	
	# scopy-on-read=	
	# sbps=, bps_rd=, bps_wr=	
	# sbps_max=, bps_rd_max=, bps_wr_max=	
	# siops=, iops_rd=, iops_wr=	
	# siops_max=, iops_rd_max=, iops_wr_max=	
	# siops_size=
	# sgroup=
-fsdev local,id=id,path=path,security_model=security_model
-fsdev proxy,id=id,socket=socket
-fsdev proxy,id=id,sock_fd=sock_fd
-fsdev synth,id=id
	# 定义一个新的文件系统设备
	# local：对文件系统的访问是由QEMU完成的
	# proxy：对文件系统的访问是由virtfs-proxy-helper完成的
	# synth：Synthetic filesystem, only used by QTests. 
-fda	# 软盘
-fdb	# 软盘
-hda FILE	# 相当于 -drive file=FILE,index=0,media=disk
-hdb FILE	# 相当于 -drive file=FILE,index=1,media=disk
-hdc FILE	# 相当于 -drive file=FILE,index=2,media=disk
-hdd FILE	# 相当于 -drive file=FILE,index=3,media=disk
-hdachs
-mtdblock
-pflash
-sd
-snapshot
```

##### USB选项

```
-usb	# 使能USB驱动器(默认是不使用USB驱动器的)
-usbdevice # 已过时，应使用-device usb-...来代替
```

##### 显示选项

```
-display spice-app
-display sdl
-display gtk[,grab_on_hover=on|off][,gl=on|off]
-display vnc=<display>
-display egl-headless
-display none
	# 选择display的后端类型，默认为'-display gtk'
	# gtk的意思是在GTK窗口中显示视频输出。该接口提供了下拉菜单和其它UI元素来控制VM。
	# none的意思是不显示视频输出。它类似于-nographic选项的作用，但不会重定向串口或并口的数据。
-full-screen	# 以全屏启动
-fnographic	# 禁用图形输出并将串行I/O重定向到控制台
-vga [cirrus|std|vmware|qxl|xenfb|tcx|cg3|virtio|none]
	# 选择显卡类型
	# cirrus指的是GD5446显卡。Windows系统从win95之后都能识别和使用此显卡。为qemu 2.2之前的默认显卡。
	# std是标准VGA显卡。为qemu 2.2之后的默认显卡。
	# none指的是没有显卡。
-vnc display[,options]	# 使用此选项，可以让qemu把vga显示重定向到vnc显示：<display>。
  host:d	# 只允许主机host通过端口d进行tcp连接。d是vnc端口，实际的tcp端口一般是5900+d。host可以省略，此时允许所有主机的连接。
```

##### i386 target only

```
-smbios file=binary
	# 从binary文件加载SMBIOS条目
-smbios type=0
	# 定义SMBIOS type 0字段
-smbios type=1
	# 定义SMBIOS type 1字段
-smbios type=2
	# 定义SMBIOS type 2字段
-smbios type=3
	# 定义SMBIOS type 3字段
-smbios type=4
	# 定义SMBIOS type 4字段
-smbios type=17
	# 定义SMBIOS type 17字段
```



##### 网络选项

```
-nic [tap|user|...] [,...] [,mac=] [,model=]	# 此选项是一个快捷方式，用来一次性配置网络的前端和后端。网络的后端的选项和-netdev的选项是一样的。
-nic none	# 意味着不配置任何网络设备。它用来覆盖默认配置。默认配置是前端为默认网卡，后端为user。当没有提供其它网络选项的时候会激活默认配置。
-netdev user,id=str[,...]	# 设置宿主网络为用户模式，这样就不需要超级用户的权限了。
  hostfwd=[tcp|udp]:[hostaddr]:hostport-[guestaddr]:guestport
  	# 把主机端口hostport重定向到客户机端口guestport
-netdev tap,id=<str>[,...]	# 设置网络后端为TAP，它的ID为str。
	fd=<h>			# 用以指定一个已经打开的TAP接口的句柄，这个句柄为<h>。
	ifname=<name>	# 指定接口的名字为<name>。如不指定，OS会自动分配一个。
	script=<file>	# 使用网络脚本<file>来配置此后端网络。默认配置脚本为/etc/qemu-ifup。如不使用配置脚本应写成"script=no"。
	downscript=<dfile>	# 使用网络脚本<dfile>来取消对此后端网络的配置。默认取消配置的脚本是/etc/qemu-ifdown。如不使用取消配置的脚本应写成"downscript=no"。
-netdev bridge,id=str[,...]
-netdev l2tpv3,id=str,...
-netdev socket,id=str[,...]	# 
-netdev vde,id=ID[,...]	# 配置VDE后端
-netdev vhost-user,id=str,...	# vhost-user网络设备
-netdev hubport,id=str,...	# 创建hub端口
-net nic[,...]	# 配置或创建网卡(即网络的前端)。古老的配置方式，建议用"-device"或"-nic"取代它。
-net [user|tap|bridge|socket][,...][,name=<name>]	＃ 配置宿主机网络的后端（其选项和对应的-netdev选项是一样的），并将其连接到虚拟hub0（默认hub）。name用来指定hub端口的名字。古老的配置方式，建议用"-netdev"或"-nic"取代它。
```

##### 字符设备选项

```
-chardev socket,id=id,path=path
```

##### 蓝牙选项

```
-bt
```

##### TPM设备选项

```
-tpmdev
```

##### 启动选项

```
-kernel		# 指定内核镜像
-append		# 指定内核启动参数
-initrd		# 指定initial ram disk
-dtb file	# 使用file作为设备树镜像
```

##### 调试选项

```
-bios FILE	# 为BIOS指定文件
-enable-kvm	# 开启KVM虚拟化支持
-gdb dev	# 等待gdb来连接设备dev。
-monitor <dev>	# 把控制台重定向到宿主机的设备dev。图形模式下默认设备是"vc"，非图形模式下默认设备是"stdio"。
-nodefaults	
	# 不要创建默认设备
-parallel DEV	# 将虚拟并口重定向到宿主机的设备DEV。
-serial DEV	# 将虚拟串口重定向到宿主机的字符设备DEV。图形模式下默认为vc，非图形模式下默认为stdio。
	# none - 不使用串口
	# pty - 伪终端
	# null - 丢弃所有串口输出
	# /dev/ttyS0 - 真实串口设备
	# file:/tmp/serial.out - 把串口输出写入文件
	# stdio - 重定向到标准输入输出
	# pipe:/tmp/serial:	- 重定向到管道，需手动创建/tmp/serial.in和/tmp/serial.out
	# udp::3333 - 重定向到udp，使用nc -u -l -p 3333 可访问
	# tcp::3333,servr,nowait - 重定向到tcp，使用telnet localhost 3333可访问
	# telnet::3333,server,nowait - 重定向到telnet，访问方式同tcp
	# unix:/tmp/serial.sock,server,nowait - 重定向到socket，用socat命令连接
	# 定向到braille
-S			# 在刚开始的时候别启动CPU（需要在管理器里输入“c”才会启动CPU）
-s			# 相当于-gdb tcp::1234，即在TCP端口1234开启一个gdbserver
```

#### QEMU monitor

QEMU的控制台。"Ctrl+Alt+2"进入控制台，"Ctrl+Alt+1"进入QEMU界面。也可用-monitor选项把控制台重定向到某个设备。

##### 辅助类命令

```
info	# 显示虚拟机的运行信息
help	# 查询某个命令的帮助信息
```

##### 调试类命令

```
gdbserver	# 启动qemu内置的gdbserver
x		# 打印虚地址的值
xp		# 打印实地址的值
print	# 数学运算或引用某寄存器的值
sum		# 计算某个内存区域的校验和
memsave	# 内存转储
```

##### 控制类命令

```
savevm	# 创建快照
loadvm	# 还原快照
delvm	# 删除快照
commit	# 将缓存中对块设备的操作，立即写到块设备上
system_reset	# 重启
system_powerdown	# 关机
stop/cont	# 进入/退出挂起状态
quit	# 退出qemu
```

##### 设备类命令

```
change	# 更改虚拟机的配置
mouse_move	# 移动鼠标
mouse_button	# 按下鼠标左键
sendkey	# 键盘操作
```



#### qemu-system-x86_64

	-cpu max	// cpu模型为max,意指开启加速器支持的所有特性
	-device rtl8139,netdev=tapnet
			//添加设备rtl8139,tapnet是做为后台的netdev的ID
	-display none	// 不显示视屏输出，注意它和-nographic的效果是不一样的。
	-drive file=arch_disk,format=raw
			//定义一个新的驱动器，本驱动使用的磁盘镜像是arch_disk,磁盘格式设为raw用来避免解释不可信的格式头
	-kernel		// 指定内核镜像。
	-m 1G		//使用1G内存
	-M q35,accel=kvm:tcg,kernel-irqchip=split
		# 使用机器类型intel q35，使用加速器kvm:tcg，加速器的in-kernel irqchip支持为split。
		# Qemu支持的Intel机器类型仅有两种，一个是i440FX，一个是Q35。i440FX诞生于1996年，是北桥芯片，其对应的南桥芯片是PIIX。Q35是2007年推出的芯片组，它即包括了北桥芯片(MCH)，也包括了南桥芯片(ICH9)。Q35相较于i440FX的最大差异在于它支持PCIe。
	-net nic	//创建一个新网卡，默认是e1000。
	-net user	//使用用户模式的网络栈，无需管理员权限。
	-net tap,ifname=tap0,script=no,downscript=no
	-netdev tap,id=tapnet,ifname=tap0,script=no,downscript=no
			//连接到主机的tap网络接口，script和downscript设置为no使脚本文件失效
	-object filter-dump,id=filter0,netdev=net0,file=o.x86_64/qemu.pcap
	-S	// 在刚开始的时候不启动CPU
	-s	// -gdb tcp::1234的缩写
	-serial mon:stdio	// 创建多路复用的stdio后端，其连接在串行端口和qemu管理器。
	-smp cpus=1		// 模拟1个CPU的SMP系统
#### qemu-system-riscv

```
-machine 	// 选择要模拟的机器，可选机器如下
  none		# 空机器
  sifive_e		 # 满足SiFive E SDK的开发板
  sifive_u		 # 满足SiFive U SDK的开发板
  spike_v1.10	 # Spike开发板(priv 1.10)
  spikes_v1.9.1	 # Spike开发板(priv 1.9.1)
  virt		# VirtIO开发板(priv 1.10)
-devece	# 对于riscv来说，可选设备如下
  # 存储设备
	virtio-blk-device：使用virtio-bus总线
		driver=<str>:指定块设备的名称
  # 网络设备
	virtio-net-device:使用virtio-bus总线
  # 输入设备
  # 显示设备
	virtio-gpu-device:使用virtio-bus总线
  # 其它设备
  # 未分类的设备
	loader:desc "Generic Loader"
	virtio-rng-device:使用PCI总线
```

##### 五种开发板的比较

- `spike_v1.9.1` - HTIF console, config-string

  实现的特权级版本为priv v1.9.1，使用config-string将设备配置传递给BBL。

- `spike_v1.10` - HTIF console, device-tree

  实现的特权级版本为priv v1.10，使用设备树将设备配置传递给BBL。

- `sifive_e` - SiFive UART, HiFive1 compat

- `sifive_u` - SiFive UART, device-tree

- `virt` - 16550A UART, VirtIO, device-tree

  VirtIO板模拟了完整的计算机系统，方便操作系统的开发。它实现了VirtIO MMIO传输，支持VirtIO块设备和VirtIO网络设备，还有16550a UART用于控制台访问。这样qemu原生的各种块设备和网络实现都可用了。
  
  通过查看`hw/riscv/virt.c`可以看到内存的布局，可以看到外设的地址，也可以看到DRAM是从`0x80000000`开始的。与virt板子相关的部分是从`virt_machine_init`开始的，首先是注册PLIC中断，然后注册系统内存，然后创建设备树。
  
  ```
  # 查看virt machine的硬件配置信息
  sudo apt install device-tree-compiler
  qemu-system-riscv64 -M virt -M dumpdtb=riscv64-virt.dtb -bios default
  dtc -I dtb -O dts -o riscv64-virt.dts riscv64-virt.dtb
  view riscv64-virt.dts
  ```
  
  

##### 设备树

描述RISC-V机器的配置。还可以包含非易失性存储的配置信息，如控制台输出信息和启动参数。可以用`-append`选项来让设备树(如根设备和控制台)传递启动命令。

##### SiFive CLINT

CLINT是单个核心的局部中断控制器，SiFive ClINT模块控制着与软件和计时器中断相关的内存映射和状态寄存器。兼容FE310G000 SOC，the SiFive Freedom-E-SDK and the SiFive Freedom-U-SDK。

##### SiFive PLIC

PLIC是平台级中断控制器。SiFive PLIC对全局中断进行优先级划分和分发。

#### qemu-img

##### 语法

```
qemu-img [standard options] <command> [command options]	# 管理镜像
```

##### 标准选项

```
-h, --help
-V, --version
-T, --trace
```



##### 命令

```
amend
bench
check	# 对磁盘镜像文件进行一致性检查
commit
compare

# 转化镜像的格式
convert	[-f fmt] [-O output_fmt] <filename> <output_filename>

# 创建镜像文件
create [-f fmt] [-b backing_file] [-F backing_fmt] [-u] [-o options] <filename> [size]	

dd
info	# 查看镜像的信息
map
measure
snapshot	# 管理镜像的快照
rebase		# 在已有镜像的基础上创建新的镜像
# 改变镜像的大小
resize [--object ojbdef] [--image-opts] [-f fmt] [--preallocation=prealloc] [-q] [--shrink] <filename> [+|-]<size>
```

##### 命令选项

```
filename	# 磁盘镜像的文件名
size	# 镜像的大小。默认单位为byte，可带后缀K,M,G,T,P,E。
-b backing_file
-F backing_fmt	# 第二个镜像的格式。
-f fmt			# 第一个镜像的格式。大多数情况下都可以自动检测
-o options	# 格式的定义。是由逗号分割的键值对的列表。使用"-o ?"查看所有支持的键值对。
--object objectdef	# 对用户可创建对象的定义。详见qemu手册
output_filename
output_fmt
-q	# 静默模式。不打印任何输出。
-u	# 允许不安全的backing链。

# snapshot子命令

# compare子命令

# convert子命令

# dd子命令
```

##### 镜像文件的格式

```
raw
qcow2
other:包括VMDK, VDI, VHD (vpc), VHDX, qcow1 and QED
```

#### 参考资料

- [Qemu4.2.0用户文档](https://qemu.weilnetz.de/doc/qemu-doc.html#Display-options)
- QEMU新的-nic选项：[中文翻译版](https://zhuanlan.zhihu.com/p/41258581)、[英文版](https://www.qemu.org/2018/05/31/nic-parameter/)
- [Qemu虚拟化之Machine Type](https://remimin.github.io/2019/07/09/qemu_machine_type/)
- [-kernel, -bios, -device loader的区别](https://stackoverflow.com/questions/58420670/qemu-bios-vs-kernel-vs-device-loader-file)

#### 问题的解决

##### 问题一

问题描述：安装好系统的镜像文件，启动的时候黑屏无反应。

解决方法：加上`-enable-kvm`选项

原因分析：不开kvm硬件加速的情况下，可能cpu执行慢或代码进入了某种死循环中。
