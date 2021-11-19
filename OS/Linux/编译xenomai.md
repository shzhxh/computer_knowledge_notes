#### 源码下载

- 对于Cobalt和Mercury，选Cobalt。因为Cobalt是真正的实时，而Mercury只是一层库的封装。
- 通过I-pipe patch与本机内核版本号的对比，选5.4.151。我感觉两者越接近，出问题的可能性越小。

```
wget https://xenomai.org/downloads/ipipe/v5.x/x86/ipipe-core-5.4.151-x86-7.patch
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.151.tar.xz
git clone https://source.denx.de/Xenomai/xenomai.git
cd xenomai && git checkout v3.2.1	# 切换到一个稳定版
	# kernel目录下是内核支持的代码，可视为Linux内核的扩展
	# lib目录下是用户空间的库，由xenomai框架把它导出到应用程序
	# scripts,testsuite,utils目录提供额外的脚本和程序，用于构建host和运行时目标。
```

#### 安装Cobalt

```sh
# Cobalt是要集成到Linux里的，scripts/prepare-kernel.sh用来进行这样的设置
# scripts/prepare-kernel.sh参数：
	# --linux : 指定Linux源码的目录。默认为$PWD.
	# --ipipe : 指定要加到Linux源码上的ipipe补丁。如已打上补丁，此参数可省略。
	# --arch : 指定目标架构的信息。默认为主机的架构。

# 1 setup patch
cd xenomai
./scripts/prepare-kernel.sh --arch=x86_64 --linux="../linux-5.4.151" --ipipe="../ipipe-core-5.4.151-x86-7.patch"

# 2 config 
make menuconfig
	# 配置实时内核的第一原则：不要盲从默认配置，而是要让配置符合真实的硬件。
	# 发行版附带的配置与实时的需求是矛盾的，尤其是双内核配置。
	# 小心网上给出的配置。比如邮件、归档文件和搜索结果。如果我们曾在邮件里说“请禁用选项X或Y”，它并不是指普遍的情况，而只是为了定位特定问题的设置。对于第三方网站上的类似建议也应持保留态度，因为作者也可能存在同样的误解。靠谱的方式是在xenomai讨论列表里提问。
	# x86处理器的类型：要精确指定处理器，而不要使用i586这样的占位符。因为通用的i586没有TSC特性，xenomai不得不模拟一个TSC，从而导致性能不佳。
	# 电源管理：不应关闭全局的电源管理，在电源管理部分能禁用的只有两个选项：CONFIG_APM和CONFIG_ACPI_PROCESSOR。注意：启用ACPI不会带来高时延的风险，但禁用它可能会导致系统启动异常。
	# USB：应启用USB主机控制器的支持，而不要禁用它。因为USB主机控制器会禁用“legacy USB enulation”，从而避免高时延。
	# CPU频率调节：应禁用CONFIG_CPU_FREQ，它会使xenomai计时代码出问题，它使实时线程的运行时不可预测，当CPU频率动态改变时它还可能引发高时延。
	# CONFIG_CPU_FREQ : 应禁用。许多CPU会随着频率的改变来改变TSC计数的频率，这对精确计数是无益的。
	# CONFIG_CPU_IDLE：应禁用。这会使CPU进入深度睡眠，从而增加idle系统的时延。
	# CONFIG_CC_STACKPROTECTOR：在xenomai2，x86_64平台上开启；在xenomai3上则没有关系。
	# CONFIG_APM：应禁用。APM模式会把电源管理的权力交给BIOS，但BIOS不能很好地控制时延。
	# CONFIG_ACPI_PROCESSOR：应禁用。它会增加系统时延，导致xenomai计时器初始化失败。
	# CONFIG_INTEL_IDLE：应禁用。它会使CPU进入深C状态从而导致巨大的时延。
	# CONFIG_INPUT_PCSPKR：应禁用。
	# CONFIG_PCI_MSI：如果interrupt lines只是来自于Linux上下文(次要模式)，而决不来自于实时模式，这个选项也可以启动。
	# 发行版的内核都是从initramfs引导的，这意味着内核不需要特别的内建支持，任何东西都可构建为模块，所以如果是发行版应为新内核生成initramfs。如果没有initramfs，就必须在内核里构建挂载根文件系统的所有组件。
	# 如果时延测试观察到了巨大的时延数据(比如几百毫秒)，对x86平台的通常考虑是SMI(系统管理中断)。调整SMI请务必谨慎，因为这有可能会损坏你的硬件。
	# 让内核设置符合硬件，可避免无用的开销或低效代码的生成。
	
# 3 编译
# 3.1 对于debian及其衍生版
make -j4 deb-pkg
sudo dpkg -i ../*.deb
# 3.2 对于任何Linux系统
make bzImage modules -j4
make modules_install	# 编译内核模块
sudo make install
sudo mkinitramfs -o /boot/initrd.img-5.4.151
sudo update-initramfs -c -k 5.4.151
sudo update-grub
```

##### 问题解决

```
# 问题一
# 问题描述
arch/x86/kernel/process.c:699:4: error: implicit declaration of function ‘__ipipe_halt_root’;
# 解决方法
make menuconfig，把PARAVIRT对应的项去掉
# 原因分析
PARAVIRT和IPIPE是矛盾的，PARAVIRT的存在可能使编译器不能正确地找到‘__ipipe_halt_root’
```

#### 安装xenomai库和工具

1 前提要求

```
# Cobalt和Mercury都需要
	# GCC必须支持原子的内建函数，即__sync_XXX
	# GCC应支持TLS
	# 如果需要用户空间的寄存器支持，则目标内核必须开启CONFIG_FUSE_FS，且工具链必须包含FUSE开发库
	# 如果是自己从git源码构建，则必须包含autoconf, automake和libtool包。
# Cobalt需要
	# 内核必须是3.10以上
	# 内核必须包含I-pipe补丁
	# x86_32处理器需要TSC支持

```

2 配置

```
./scripts/bootstrap	# 生成脚本文件configure
# configure脚本用于为下步的编译作好准备，它可接收的参数如下：
## Cobalt和Mercury都适用的参数：
--with=core=<type>	# 为哪个实时核构建库的支持，可选cobalt或mercury。
--enable-pshared	# 允许多处理器共享实时对象(任务，信号量等)
--enable-smp	# 开启SMP支持。如果要运行在支持SMP的内核上必须开启此选项。
## Cobalt适用的参数：
## Mercury适用的参数：
```

3 交叉编译

```
# 要支持交叉编译，要向configure传递--host和--build两个参数
# 注意给交叉编译工具加上前缀，不过--host会默认加上个前缀
# 使用交叉编译器的最简单的方式是crosstool-ng
# 如果不想自己构建交叉编译器，可以使用ELDK
```

4 编译

```sh
# x86(32/64), Cobalt库
mkdir build && cd build
../configure --with-core=cobalt --enable-smp --enable-pshared
sudo make install
```

5 环境配置

```
vim /etc/profile.d/xenomai.sh	# 输入如下内容
# xenomai的环境变量
export XENOMAI_ROOT_DIR=/usr/xenomai
export XENOMAI_PATH=/usr/xenomai
export PATH=$PATH:$XENOMAI_PATH/bin:$XENOMAI_PATH/sbin
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:$XENOMAI_PATH/lib/pkgconfig
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$XENOMAI_PATH/lib
export OROCOS_TARGET=xenomai

# xenomai快捷命令
alias xeno-stat="cat /proc/xenomai/sched/stat"
alias xeno-threads="cat /proc/xenomai/sched/threads"
alias xeno-rt-threads="cat /proc/xenomai/sched/rt/threads"
alias xeno-interrupts="cat /proc/xenomai/irq"
alias xeno-version=/usr/xenomai/sbin/version
alias xeno-autotune=/usr/xenomai/sbin/autotune
alias xeno-latency=/usr/xenomai/bin/latenc
```

6 增加用户组

```
$sudo addgroup xenomai --gid 1234	# 创建用户组xenomai
$sudo addgroup root xenomai			# root用户加入xenomai组
$sudo usermod -a -G xenomai $USER	# 当前用户加入xenomai组
```

7 配置grub

```
vim /etc/default/grub	# 添加用户组信息xenomai.allowed_group=1234，形式如下：
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash xenomai.allowed_group=1234"
sudo update-grub	# 更新grub
```

##### 问题解决

```
# 问题一
# 问题描述
configure: error: C compiler cannot create executables
# 解决方法
执行configure脚本的时候去掉CFLAGS和LDFLAGS
# 原因分析
网上有人说是修改CFLAGS环境变量引起的，原因未知
```

