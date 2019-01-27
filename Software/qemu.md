#### 安装
```shell
./configure --target-list=riscv32-softmmu
sudo make install
```
> ERROR: glib-2.22 gthread-2.0 is required to compile QEMU
 `sudo apt install libglib2.0-dev`
> ERROR: pixman >= 0.21.8 not present.
 `sudo apt install libpixman-1-dev`
#### 语法

```
qemu-system-riscv64 [options] [disk_image]
```

#### 选项

##### 标准选项

```
-h	# 打印帮助信息
-version	# 打印版本信息
-machine [type=]name[,prop[=value][,...]]	# 选择要模拟的机器
-cpu	# 选择CPU模型
-accel	# 选择加速器的模型
-smp	# 模拟多核CPU
-device driver[,prop[=value][,...]]		# 添加设备(基于driver)，prop=value是设置driver的属性
-boot # 定义启动顺序
-m	# 指定内存大小，默认128M。
-soundhw	# 使能并使用声卡。
```



##### 块设备选项

```
-fda	# 软盘
-fdb	# 软盘
-hda FILE	# 相当于 -drive file=FILE,index=0,media=disk
-hdb FILE	# 相当于 -drive file=FILE,index=1,media=disk
-hdc FILE	# 相当于 -drive file=FILE,index=2,media=disk
-hdd FILE	# 相当于 -drive file=FILE,index=3,media=disk
-cdrom FILE	# 相当于 -drive file=FILE,index=2,media=cdrom
-blockdev
-drvie option[,option[,option[,...]]]
	# -drive可以使用-blockdev的所有选项，另外，还可以使用如下选项：
	file=FILE	# 指定磁盘镜像
	if=			# 指定接口类型，可用类型有ide,scsi,sd,mtd,floppy,pflash,virtio,none
	bus= ,unit=	# 通过bus number和unit id指定连接位置
	index=	# 通过编号指定连接位置
	media=	# 指定媒介类型，disk或cdrom
	cyls=, heads=, secs= [,trans=]	#
	snapshot=	#
	cache=	#
	aio=	#
	format=	#
	serial=	#
	addr=	#
	werror=, rerror=	#
	copy-on-read=	#
	bps=, bps_rd=, bps_wr=	#
	bps_max=, bps_rd_max=, bps_wr_max=	#
	iops=, iops_rd=, iops_wr=	#
	iops_max=, iops_rd_max=, iops_wr_max=	#
	iops_size=
	group=
-mtdblock
-sd
-pflash
-snapshot
-hdachs
-fsdev

```

##### USB选项

```
-usb	# 使能USB驱动器(默认是不使用USB驱动器的)
-usbdevice # 已过时，应使用-device usb-...来代替
```

##### 显示选项

```
-display
-nographic	# 禁用图形输出并将串行I/O重定向到控制台
-vga TYPE	# 模拟VGA显卡，可选的TYPE有cirrus, std, vmware, qxl, tcx, cg3, virtio, none
```

##### 网络选项

```
-netdev user,id=str[,...]	# 设置宿主网络为用户模式，这样就不需要超级用户的权限了。
-netdev tap,id=str[,...]	# 设置宿主网络为tap模式
-netdev bridge,id=str[,...]
-netdev l2tpv3,id=str,...
-netdev socket,id=str[,...]	# 
-netdev vde,id=ID[,...]	# 配置VDE后端
-netdev vhost-user,id=str,...	# vhost-user网络设备
-netdev hubport,id=str,...	# 创建hub端口
-net nic[,...]	# 配置或创建网卡
-net [user|tap|bridge|socket][,...]	＃ 指定连接方式，和对应的-netdev选项是等价的
```

##### 字符设备选项

```
-chardev
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
-serial DEV	# 将虚拟串口重定向到宿主机的字符设备DEV。图形模式下默认为vc，非图形模式下默认为stdio。
-parallel DEV	# 将虚拟并口重定向到宿主机的设备DEV。
-bios FILE	# 为BIOS指定文件
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
		// 使用机器类型intel q35，使用加速器kvm:tcg，加速器的in-kernel irqchip支持为split。
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
  # none	空机器
  # sifive_e 满足SiFive E SDK的开发板
  # sifive_u 满足SiFive U SDK的开发板
  # spike_v1.10 Spike开发板(priv 1.10)
  # spikes_v1.9.1 Spike开发板(priv 1.9.1)
  # virt	VirtIO开发板(priv 1.10)
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

##### 设备树

描述RISC-V机器的配置。还可以包含非易失性存储的配置信息，如控制台输出信息和启动参数。可以用`-append`选项来让设备树(如根设备和控制台)传递启动命令。

##### SiFive CLINT

CLINT是单个核心的局部中断控制器，SiFive ClINT模块控制着与软件和计时器中断相关的内存映射和状态寄存器。兼容FE310G000 SOC，the SiFive Freedom-E-SDK and the SiFive Freedom-U-SDK。

##### SiFive PLIC

PLIC是平台级中断控制器。SiFive PLIC对全局中断进行优先级划分和分发。

#### qemu-img

##### 语法

```
qemu-img [standard options] command [command options]
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
convert	# 转化镜像的格式
create	# 创建镜像文件
dd
info	# 查看镜像的信息
map
measure
snapshot	# 管理镜像的快照
rebase		# 在已有镜像的基础上创建新的镜像
resize		# 改变镜像的大小
```

##### 命令选项

```
filename
fmt
size
output_filename
output_fmt
```

##### 镜像文件的格式

```
raw
qcow2
other:包括VMDK, VDI, VHD (vpc), VHDX, qcow1 and QED
```



