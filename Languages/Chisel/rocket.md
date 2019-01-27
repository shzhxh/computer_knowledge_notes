#### 简介

Rocket是一个开源的System-on-Chip设计生成器，用以生成可综合的RTL (所以Rocket不是单个的设计实例，而是一个设计实例的生成器，可以生成多种设计实例)。

Rocket是用的Chisel语言写的，一种嵌入在Scala语言里的开源硬件构建语言。它构建了一个RISC-V的平台。它是由一些参数化的构建库组成的，就是用这些构建库来生成各种各样的RTL实例的。

Rocket生成通用处理器的核，它以RISC-V为ISA，其中**Rocket**生成有序的核，**BOOM**生成无序的核。

#### 仓库内容

##### 子模块

- [chisel3](https://github.com/ucb-bar/chisel3)：用来生成RTL(Register-Transfer Level)
- [firrtl](https://github.com/ucb-bar/firrtl)：(Flexible Internal Representation for RTL)，是RTL的中间表示，由Chisel3来使用，通过它来生成最终代码(Verilog, C等)，详细介绍见[specification][6]
- [hardfloat](https://github.com/ucb-bar/berkeley-hardfloat)：浮点单元的Chisel代码，用于融合的乘加运算、整数浮点数转换、不同精度的浮点数转换
- [riscv-tools](https://github.com/riscv/riscv-tools)：riscv软件生态环境，与本仓库中的RTL一起工作
- [torture](https://github.com/ucb-bar/riscv-torture)：生成和执行随机指令流，用于对本设计进行压力测试

##### scala包

在`src/main/scala`目录下

- amba：这个RTL包是AMBA协议的总线实现，包括AXI4, AHB-lite, APB
- config：通过动态范围的参数化库来配置生成器
- coreplex：将其它包的各种组件粘合在一起，生成一个完整的coreplex
- devices：外围设备的实现，包括调试模块和各种TL从设备。
- diplomacy：通过两阶段的硬件细化来扩展chisel，两个阶段的交互详情见[paper][7]
- groundtest：生成可综合的硬件测试器，通过发出随机内存访问来对非核内存进行压力测试
- jtag：生成JTAG总线接口的定义
- regmapper：生成带有标准化接口的从设备，用于访问其内存映射的寄存器
- rocket：生成rocket核和L1缓存
- tile：包含能与核心相结合来构建tiles的组件，比如FPU和加速器。
- tilelink：用于生成Tilelink协议的总线实现，还包含了各种适配器和协议转换器。
- system：用Chisel详述coreplex的特定配置
- unittest：用于生成单个模块的硬件测试器
- util：可供其它包重用的组件

##### 其它资源

- bootrom：BootROM中的first-stage bootloader源代码
- csrc：用于Verilator模拟的C源代码
- emulator：编译和运行Verilator
- project：由sbt使用，用于scala编译和构建
- regression：持续集成和nightly regression套件
- scripts：解析模拟输出或操作源文件
- vsim：编译和运行Synopsys VCS
- vsrc：verilog源文件，包含接口、harnesses和VPI

##### 扩展Top-Level设计

详见[project-template](https://github.com/ucb-bar/project-template)

#### 使用FPGA

##### 编译rocket

- 约定：**ROCKET_DIR**为rocket-chip所在的目录
- 约定：**RISCV_DIR**为riscv所安装的目录

```shell
# 1. 下载rocket源码
git clone https://github.com/ucb-bar/rocket-chip.git
cd rocket-chip
git submodule update --init
# 2. 编译riscv环境(因为对应了特定的提交版本，所以即使有编译好的环境也需要重新编译) 
export RISCV=$RISCV_DIR
cd $ROCKET_DIR/riscv-tools
git submodule update --init --recursive
export MAKEFLAGS="$MAKEFLAGS -j8"
./build.sh
# 3. 编译生成verilog(生成的代码在$ROCKET_DIR/vsim/generated-src)
cd $ROCKET_DIR/vsim
sudo apt install openjdk-8-jdk git device-tree-compiler	# need to install first
make verilog CONFIG=DefaultFPGAConfig
  # 在使用以上命令时会出现大量unable to get resource的错误提示
  # this is a bug with ubuntu 18.04, when I use 16.04, it resolved.
# 4. 保持仓库更新
cd $ROCKET_DIR
git pull origin master
git submodule update --init --recursive
  # 如果riscv-tools的版本改变了，需要重新编译
  cd riscv-tools
  ./build.sh
```

##### 使用FPGA

```
git clone https://github.com/ucb-bar/fpga-zynq
git cd fpga-zynq
cd zedboard
make init-submodules	# 初始化子模块
make rocket		# 生成新代码
make project	# 建立vivado项目
```



##### 使用xxx

##### 使用VLSI工具

#### 参数化

#### 使用GDB调试

#### 参考资源

[1]:https://github.com/ucb-bar/fpga-zynq "在zynq FPGA开发板上跑RISC-V所需要的文件"
[2]:https://github.com/freechipsproject/rocket-chip "rocket源码"
[3]:https://www2.eecs.berkeley.edu/Pubs/TechRpts/2016/EECS-2016-17.pdf "rocket的详细说明文档"
[4]:http://www.lowrisc.org/docs/tagged-memory-v0.1/rocket-core/	"rocket core overview"
[5]:http://www.lowrisc.org/docs/tagged-memory-v0.1/rocket-chip/	"rocket chip overview"
[6]:https://www2.eecs.berkeley.edu/Pubs/TechRpts/2016/EECS-2016-9.pdf	"Specification for the FIRRTL Language"
[7]:https://carrv.github.io/2017/papers/cook-diplomacy-carrv2017.pdf	"cook-diplomacy-carrv2017"
