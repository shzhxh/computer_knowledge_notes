#### 简介

建立基本的debian系统。

```bash
debootstrap [options] <suite> <target> [mirror [script]]
	# suite : debian的版本号，可以是释放代码的名字(如sid, stretch, jessie)或符号名(如unstable, testing, stable, oldstable)
	# target : 安装debian的目录
	# mirror : debian的镜像源
	# script : 安装脚本
debootstrap [options] --second-stage	# 完成创建过程
```

#### 选项

```
--arch=<ARCH>	#
--include=<alpha, beta>	#
--exclude
--components
--no-check-gpg	# 不检查gpg签名
--no-resolve-deps
--variant=

```

#### 示例

```bash
# 创建riscv64的chroot环境
sudo apt-get install debootstrap qemu-user-static binfmt-support debian-ports-archive-keyring
sudo debootstrap --arch=riscv64 --keyring /usr/share/keyrings/debian-ports-archive-keyring.gpg --include=debian-ports-archive-keyring unstable /tmp/riscv64-chroot http://deb.debian.org/debian-ports
```



#### 错误解决

##### 1 Cannot check Release signature

使用`--no-check-gpg`选项，不检查gpg签名

##### 2 Failed getting release file