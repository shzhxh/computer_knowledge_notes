#### 简介

gcc一般会进行预处理，编译，汇编，连接四步。

#### gcc选项

##### 通用选项

```
-c			# 不运行连接器，这样就只生成汇编器生成的目标文件。
-S|-E	#
-std=standard	#
-g			#
-pg			#
-w			# 不生成警告信息
-Wall		# 生成所有警告信息
-Idir		# 在dir目录内寻找头文件
-Ldir		# 指定库所在的目录。如果库不在系统库的目录下，需要使用此参数。
-fno-buitin	# 不使用C语言的内建函数
-ggdb		# 尽可能生成gdb调试信息
-m32		# 生成32位代码
-nostdinc	# 不使用系统缺省头文件目录
-fno-stack-protector	# 禁用堆栈保护
-O0		# 不进行优化
-O1或-O	# 缺省优化
-O2		# 比-O1高一级的优化
-Os		# 使用所有-O2优化，但不缩减代码尺寸
-O3		# 最高级别优化
-T		# 指定链接脚本。在没有操作系统的裸板上，可能需要-T选项来避免对未定义符号的引用。可以在搜索“lds链接脚本”关键字得到更详细信息。
-l		# 指定要链接的库。默认为系统的库的路径。可查看/etc/ld.so.conf文件获取系统库路径的详情。
```

##### C语言选项

##### C++语言选项

##### Objective-C和Objective-C++语言选项

##### 诊断信息格式选项

##### 请求或禁止Warning选项

```
-Werror		# 让所有的警告都变成错误
-Wunused-function	# 
-Wunused-label		# 当一个标签只声明而不使用时进行警告
-Wunused-parameter	# 当函数参数未使用时进行警告
-wunused-variable	# 变量只定义而未使用时进行警告
-Wunused-value		# 当一个语句的计算结果未使用时进行警告
-Wunused			# 所有-Wunused-*选项加到一块
```



##### C和Objective-C专用Warning选项

##### 调试选项

##### 优化选项

##### 程序工具选项

##### 预处理器选项

```
-Dmacro		# 预定义宏macro，其值默认为1
-M			# 输出依赖关系的规则，而不是预处理的结果
-MM			# 类似于-M，但不提示系统目录里的头文件
-MF file	# 与-M或-MM一块使用时，将依赖写入文件file
-MD			# 相当于-M -MF file，只是-E不是隐含的。file的确定依赖于-o选项。
-MMD		# 和-MD一样，但只提示用户头文件，不提示系统头文件。
```



##### 汇编器选项

##### 连接器选项

##### 文件夹选项

##### 代码生成选项

##### 开发者选项

##### 机器相关选项

##### 控制输出类型的选项

##### RISC-V选项

```bash
# for GCC7.3.0
-mbranch-cost=n
  # 将branch的cost设置为大约n条指令。
-mplt, -mno-plt
  # 当生成PIC代码时，是否允许使用PLTs。默认允许。
-mabi=ABI-string
  # 指定整数和浮点调用规约。ABI-string由两部分组成：整数类型的大小和浮点型的寄存器。如-mabi=lp64d指long和指针是64位，使用d扩展的寄存器(64位)；-mabi=lp64f指long和指针是64位，使用f扩展的寄存器(32位)；-mabi=lp64指long和指针是64位，不使用浮点寄存器。这个参数的默认值是系统决定的，用户如果想使用特定的调用约定就要明确指定出来。有效的调用约定有：ilp32, ilp32f, ilp32d, lp64, lp64f, lp64d。要注意一些调用约定在某些ISAs上是不可能实现的:例如，-march=rv32if -mabi=ilp32d是无效的，因为ABI需要在F寄存器中传递64位值，但是F寄存器只有32位宽。
-mfdiv, -mno-fdiv
  # 是否使用浮点除法或开方指令。这要求F或D扩展。默认如果硬件支持那些指令就使用它们。
-mdiv, -mno-div
  # 是否使用整数除法的指令。这要求M扩展。默认如果硬件支持那些指令就使用它们。
-march=ISA-string
  # 为给定RISC-V ISA生成代码。ISA-string要求小写。如rv64i,rv32g, rv32imaf等。
-mtune=processor-string
  # 优化通过微架构名定义的给定处理器的输出
-msmall-data-limit=n
  # 将小于n字节的全局和静态数据放到一个特定的section。
-msave-restore, -mno-save-restore
  # 是否使用更小但更慢的使用函数调用的开始和结束代码。默认是使用快速的内联函数来实现开始和结束代码。
-mstrict-align, -mno-strict-align
  # 是否生成未对齐的内存访问。默认设置取决于我们正在优化的处理器是否支持快速的未对齐访问。
-mcmodel=medlow
  # 为medium-low代码模型创建代码。要求程序必须在2G地址范围内，且是-2G到2G之间。这是默认的代码模型。
-mcmodel=medany
  # 为medium-any代码模型创建代码。要求程序必须在2G地址范围内。
-mexplicit-relocs, -mno-exlicit-relocs
  # 在处理符号地址时是否使用汇编程序迁移操作符
```



#### gcc使用过程错误解决

**错误提示**：error while loading shared libraries: libncursesw.so.5

原因分析：原因一，新系统使用旧系统编译出来的文件，但两个系统支持的库版本不一致，所以引发的这个问题。原因二，网上有人说是系统升级不完全，需要换源。

解决方法：对于原因一，要把需要的二进制文件重新编译一遍。对于原因二，要换源再升级系统，看看所缺的库是否回来了。

**错误提示**：fatal error: asm/socket.h: 没有那个文件或目录

原因分析：ubuntu将asm文件夹改名为了asm-generic，所以找不到asm文件夹了  

解决方法：创建一个链接asm指向文件夹asm-generic

**错误提示**：Instruction csrw requires absolute expression

原因分析：这是由于编译器版本不正确引起的，由于riscv指令集尙处于发展阶段，所以不同版本的指令集可能是不一样的

解决方法：重新编译工链，要使用指令集对应的编译器版本号

**错误提示**：undefined reference to `__alltraps'

原因分析：没把包含`__alltraps`的源文件加入编译系统，导致链接器找不到对`__alltraps`的引用

解决方法：修改Makefile，把相应源文件加入编译系统

**错误提示**：a label can only be part of a statement and a declaration is not a statement

原因分析：一个标签的后面只能跟语句而不能跟声明，我的问题在于switch语句里声明了一个变量

解决方法：把变量声明放到switch语句的外面

**错误提示**：multiple definition of XXX

原因分析：我碰到的问题是在头文件里实现了函数，当多个二进制文件链接到一块的时候会产生冲突

解决方法：在头文件里声明函数，在其它c文件里实现这个函数。变量定义应该也会产生这个问题，所以不要在头文件里定义变量，而只声明变量。

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

