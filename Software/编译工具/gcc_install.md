注：我发现(201908)系统默认提供了riscv gnu toolchain，所以以后不用再自己编译它们了。本文档仅为存档记录。

#### 安装gcc-riscv32(Ubuntu 16.04 x86_64)

##### 准备

软件包：

```shell
# autoconf, automake and autotools-dev : tools to form Makefile
# curl : a tool to transfer data from or to a server
# libmpc-dev : multiple precision complex floating-point library development package
# libmpfr-dev : multiple precision floating-point computation developers tools
# libgmp-dev : Multiprecision arithmetic library developers tools
# gawk : a tool to find and replace text
# build-essential : informational list of packages which are considered essential for building packages.
# bison, flex : tools to create a compiler.
# texinfo : a documentation system that uses a single source file to produce both on-line information and printed output
# gperf : generate a perfect hash function from a key set
# libtool : Generic library support script
# patchutils : Utilities to work with patches
# bc : GNU bc arbitrary precision calculator language
# zlib1g-dev : compression library for development
$ sudo apt install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev
```

##### 安装

```bash
git clone https://github.com/riscv/riscv-tools.git
# When the project with git clone comes with the submodule, the contents of the submodule are not automatically downloaded at the beginning, so it needs the following command to download the essential submodule.
$ git submodule update --init --recursive
# Define the location of object file, e.g. /opt/riscv/toolchain
$ export RISCV=/path/to/install/riscv/toolchain
# Build the rv32g toolchain
$ ./build-rv32g.sh	# 注：使用--enable-linux将生成rv32-unknown-linux-gnu-*这样的工具链，--with-abi参数已失效，--with-arch要以RV32I开头
```

##### 错误解决

If it mentions error like this：

> configure: error: Building GCC requires GMP 4.2+, MPFR 2.4.0+ and MPC 0.8.0+.

Then GMP, MPFR and MPC should be installed:

```shell
# Download the latest version from ftp://gcc.gnu.org/pub/gcc/infrastructure
# When I download, it is mpc-1.0.3, mpfr-3.1.4, gmp-6.1.0
# 1, gmp MUST be first to build
cd gmp-6.1.0
./configure --prefix=/opt/gmp-6.1.0
make
sudo make install
make check
# 2, mpfr MUST be the second to build
cd mpfr-3.1.4
./configure --prefix=/opt/mpfr-3.1.4 --with-gmp=/opt/gmp-6.1.0
make
sudo make install
# 3, mpc MUST be the last to build
cd mpc-1.0.3
./configure --prefix=/opt/mpc-1.0.3 --with-gmp=/opt/gmp-6.1.0 --with-mpfr=/opt/mpfr-3.1.4
make
sudo make install
# 4, after that, we should modify build-rv32g.sh to tell the location of gmp,mpfr,mpc
build_project riscv-gnu-toolchain --prefix=$RISCV --with-arch=rv32g --with-abi=ilp32d --with-gmp=/opt/gmp-6.1.0 --with-mpc=/opt/mpc-1.0.3 --with-mpfr=/opt/mpfr-3.1.4 --enable-linux
```

Then run `./build-rv32g.sh` again.

#### 安装riscv-tools(Ubuntu 18.04)

##### 准备

除安装gcc-riscv32的软件包外，还需要安装device tree compiler

```bash
sudo apt install device-tree-compiler libipt-dev babeltrace expat
```

##### 安装

```bash
./build.sh
```



##### 错误解决

错误提示：configure: WARNING: libusb-1.x not found, trying legacy libusb-0.1 as a fallback; consider installing libusb-1.x instead

解决方法：

`sudo apt install libusb-1.0-0-dev`

错误提示：Makefile.am : mdate-sh and texinfo.tex not found.

解决方法：

```bash
cd riscv-tools/riscv-openocd
automake --add-missing
```



#### 安装riscv-tools(Arch Linux)

##### 准备

```bash
sudo pacman -S dtc patchutils bc
```



##### 安装

```bash
export RISCV=/path/to/install/riscv/toolchain
./build.sh
```



##### 错误解决

错误提示：`error: invalid use of incomplete type ‘scm_t_port {aka struct scm_t_port}’`

原因分析：新版本的guile不再提供对scm_t_port的定义

解决方法：禁用guile，或者使用旧版本的guile。[详细信息](https://forums.sifive.com/t/freedom-e-sdk-toolchain-fails-to-build/530/7)

```bash
vim riscv-gnu-toolchain/riscv-binutils-gdb/gdb/configure
# 将with_guile=auto改为with_guile=no
```

