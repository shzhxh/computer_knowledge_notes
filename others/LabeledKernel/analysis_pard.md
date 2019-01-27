#### 各目录的作用

##### 包含子模块的目录

- **riscv-tools**：编译工具链，gcc版本为6.1.0

- **hardfloat**：硬件浮点单元，用chisel编写。用于浮点运算，整数和浮点数之间的转换，不同数度浮点数之间的转换。

- **context-dependent-environments**：上下文依赖环境的Scale库。采用的是key-value方案，优于现有的参数化方案。

- **torture**：RISCV的测试生成器与框架。

- **chisel3**：基于FIRRTL的硬件设计语言。FIRRTL是一种用于数字电路的中间表示，它被设计成用于编写电路级转换的平台。

- **firrtl**：FIRRTL是一种用于数字电路的中间表示，它被设计成用于编写电路级转换的平台。这个仓库里包含了用Scala写的转换集合，用于简化、验证、转换或释放它们的输入电路。

##### 其它目录

- **sbt-launch.jar**：(Simple Build Tool)SBT是一个现代构建工具。它是用Scala编写的，并且针对Scala提供了很多方便快捷的功能。

- **bootrom**：写入ROM的启动代码。

- **csrc**：C++写的源文件。

- **emulator**：模拟器。

- **fpga**：主控目录。

- **project**：工程定义文件。

  - project/build.properties：声明使用哪个版本的SBT来编译当前项目
  - project/build.scala：主要的工程定义文件
  - project/plugins.sbt：声明使用哪些插件来增强SBT的功能

- **regression**：错误回退。

- **scripts**：一些脚本。

- **src/main**：pard的实现代码。

  - src/main/scala：用scala语言实现的代码。

- **target**：自动生成的目录，存放编译结果。

- **vsim**：用Verilog写的RTL模拟器，用于运行各种功能测试。

- **vsrc**：Verilog写的源文件。

#### 编译的过程

##### 1. 生成xpr文件

> xpr文件是vivado的项目文件，vivado用xpr文件来打开一个项目。

`make project PRJ=myproject BOARD=zedboard`命令实际上最终是靠`vivado $(VIVADO_FLAG) -mode batch -source board/$(BOARD)/mk.tcl -tclargs $(PRJ_FULL)`生成项目文件*.xpr的。

- `-mode batch`意思是用批处理的方式启动vivado，运行后直接执行由`-source`指定的tcl文件
- `-source board/$(BOARD)/mk.tcl`意思是指定tcl文件，Tcl是一种脚本语言
- `-tclargs $(PRJ_FULL)`意思是传给`tcl argc argv`的参数。

##### 2. 生成bit文件

vivado里打开上步生成的项目文件*.xpr，经过`Synthesis->Implementation->Bitstream`，生成system_top.bit。

依据*.xpr文件的内容推测pard源文件在以下目录：

- fpga/board/zedboard/rtl
- fpga/pardcore/rtl/rocket
- fpga/lib/include
- fpga/lib/jtag
- fpga/build/myproj_zedboard/myproj_zedboard.srcs/source_1/bd

##### 3. 生成fsbl.elf文件

> fsbl: first stage boot loader,是zynq启动第一阶段的加载程序。Boot room引导fsbl，fsbl再引导裸机程序或u-boot。

vivado里使用`export hardware`导出硬件文件system_top.hdf，再由`hsi`命令利用system_top.hdf文件生成excutable.elf文件，然后把excutable.elf重命名为fsbl.elf。

> hsi是vivado SDK附带的命令。hsi:Hardware Software Interface，用以生成fsbl.elf文件

`hsi -nojournal -nolog -source gen-fsbl.tcl -tclargs ../../../build/myproject-zedboard/myproject-zedboard.sdk/system_top.hdf`

- `-nojournal`的意思是不创建journal文件
- `-nolog`的意思是不创建log文件
- `-source arg`的意思是指定tcl文件，arg即为tcl文件的文件名
- `-tclargs arg`的意思是传递给`tcl argc argv`的参数

##### 4. 编译u-boot

##### 5. 生成boot.bin

将fsbl.elf，u-boot.elf，system_top.bit文件合成一个boot文件。这实际上是通过以下命令实现的：`bootgen -arch zynq -image bootgen.bif -w -o i build/BOOT.BIN`

- `-arch zynq`的意思是Xilinx的架构为zynq
- `-image bootgen.bif`的意思是Input Boot Image File。
- `-w`的意思是Overwrite mode，选项有：on和off。
- `-o`的意思是指定输出文件的文件名。

##### 6. 编译system.dtb

首先，用hsi命令生成dts文件。

`hsi -nojournal -nolog -source gen-dts.tcl -tclargs ../../../build/myproject-zedboard/myproject-zedboard.sdk/system_top.hdf`

其次，用dtc命令生成dtb文件。

`dtc -I dts -O dtb -o system.dtb system-top.dts`

> dtc : Device Tree Compiler，输入指定格式的设备树，输出可以启动内核的设备树。一般来说，输入的是人类可读的dts格式，而输出的是dtb格式或二进制格式。

- `-I <input format>`指定输入文件的格式，可选的格式有：dts(device tree source text), dtb( device tree blob), fs( /proc/device-tree style directory).
- `-O <output format>`指定输出文件的格式，可选的格式有：dts, dtb, asm(assembler source).
- `-o <output file>`将结果输出到文件，而不是stdout。

##### 7. 编译zImage

