

### 参考资料

- [wiki.debian.org](https://wiki.debian.org/RISC-V#Setting_up_a_riscv64_virtual_machine)

### 安装Debian(rv64)

#### 0. 环境准备

##### 0.1 工具链

```
sudo apt install gcc-riscv64-linux-gnu
```

##### 0.2 qemu

```
git clone https://github.com/qemu/qemu.git
cd qemu
./configure --target-list=riscv64-softmmu
sudo make install
./configure --target-list=riscv64-linux-user
sudo make install
```

##### 0.3 创建chroot环境

```
# 注意！！！需要在debian testing版上才可成功创建chroot环境。
sudo apt install mmdebstrap qemu-user-static binfmt-support debian-ports-archive-keyring
sudo mmdebstrap --architectures=riscv64 --include="debian-ports-archive-keyring" sid /tmp/riscv64-chroot "deb http://deb.debian.org/debian-ports/ sid main" "deb http://deb.debian.org/debian-ports/ unreleased main"
sudo chroot /tmp/riscv64-chroot
passwd
exit
```



#### 1. 编译kernel和BBL

```
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.16.tar.xz
git clone https://github.com/riscv/riscv-pk

cd linux
make CROSS_COMPILE=riscv64-linux-gnu- ARCH=riscv defconfig
make CROSS_COMPILE=riscv64-linux-gnu- ARCH=riscv menuconfig
	# Kernel hacking -> Compile-time checks and compiler options -> Compile the kernel with debug info	加入这个选项后可以用gdb调试kernel
make CROSS_COMPILE=riscv64-linux-gnu- ARCH=riscv -j8

cd ../riscv-pk/
mkdir build
cd build
../configure --prefix=/tmp --host=riscv64-linux-gnu --with-payload=../../linux/vmlinux
# 编译pk的时候，如果提示没有对__stack_chk_guard的引用，需要在Makefile的CFLAGS里加-fno-stack-protector
# 编译pk的时候，如果提示没有对__vsnprintf_chk的引用，需要在Makefile的CFLAGS里加-U_FORTIFY_SOURCE
make
cd ../../
```

#### 2. 创建镜像

```
qemu-img create rootfs.img 10G
/sbin/mkfs.ext4 rootfs.img

mount rootfs.img /mnt -o loop
cp -r /tmp/riscv64-chroot/* /mnt
umount /mnt
```

#### 3. 在qemu中运行

```
qemu-system-riscv64 -nographic -machine virt -m 2G -kernel riscv-pk/build/bbl \
 -object rng-random,filename=/dev/urandom,id=rng0 -device virtio-rng-device,rng=rng0 \
 -append "console=ttyS0 rw root=/dev/vda" \
 -device virtio-blk-device,drive=hd0 -drive file=rootfs.img,format=raw,id=hd0 \
 -device virtio-net-device,netdev=usernet -netdev user,id=usernet,hostfwd=tcp::22222-:22
```

#### 4. 设置网络

```
# 编辑/etc/network/interfaces，重启系统以开启dhcp
    auto eth0
    allow-hotplug eth0
    iface eth0 inet dhcp
```

#### 5. 更新源

```
# 编辑/etc/apt/sources.list
deb http://ftp.ports.debian.org/debian-ports/ sid main
deb http://ftp.ports.debian.org/debian-ports/ unreleased main
deb-src http://ftp.ports.debian.org/debian-ports/ sid main

# 更新源
apt update
apt upgrade
```

#### 6. 静态编译helloworld

```
riscv64-linux-gnu-gcc hello.c -static -o hello
```

#### 7. 静态编译busybox

```
wget https://busybox.net/downloads/busybox-1.30.0.tar.bz2
tar xjf busybox-1.30.0.tar.bz2
cd busybox-1.30.0
make defconfig		# 使用默认配置生成.config
make menuconfig		# 调整.config
	# Settings->Build Options选择Build static binary，使用静态编译
make CROSS_COMPILE=riscv64-linux-gnu- ARCH=riscv -j8 install
```

### 配置

```
# 安装缺失的固件
sudo apt install isenkram-cli
sudo isenkram-autoinstall-firmware

# 安装输入法
sudo apt install fcitx5 fcitx5-chinese-addons
```



### 问题解决

1. 问题描述：libwacom9 : Depends: libwacom-common (= 2.2.0-1) but 1.12-1 is to be installed

   原因分析：依赖冲突？

   解决方法：`apt install libwacom9 libwacom2-`
   
2. 问题描述：`man gcc`没有相关内容

   原因分析：gcc-doc没有安装

   解决方法：`apt instal gcc-doc`
