注：不成功

#### 安装

##### 0. 环境

`ubuntu 16.04 x86_64`

##### 1. 下载源代码

```shell
git clone https://github.com/riscv-rust/riscv-rust-toolchain.git
cd riscv-rust-toolchain
git submodule update --init --recursive
```

##### 2. 安装依赖

```shell
sudo apt install autoconf automake bash bison cmake coreutils curl device-tree-compiler diffutils doxygen expat file findutils flex gawk git libgmp-dev make grep gzip libedit2 libssh2-1 libtool libusb-1.0-0 libxml2 mpc libmpfr4 ninja libncurses5 openssl patchelf pkg-config python2.7 sed swig tar texinfo zlib1g zsh

# 需要gcc-6
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install gcc-6 g++-6
```

##### 3. 编译

```shell
vim build-env.sh	# 可选：修改安装目录，否则默认会安装在当前目录
sudo make toolchain
```

