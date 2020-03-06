### Chisel教程

#### Chisel安装

##### 安装开发工具

##### 设置本教程

本教程的目录结构：

```
build.stb	# 工程描述
run-examples.sh	# 用于执行一个或多个例子的shell脚本
run-problem.sh	# 用于执行一个或多个问题的shell脚本
run-solution.sh	# 用于执行一个或多答案的shell脚本
doc
src/main/scala
  examples	# 更多的高级电路的例子
  problems	# 需要用户来完成的一系列问题电路
  solutions	# 问题电路的答案
src/test
  resources	# 一些测试需要的资源
  scala/examples	# 例子电路的测试工具
  scala/problems	# 问题电路的测试工具
  scala/solutions	# 答案电路的测试工具
  scala/util		# TutorialRunner.scala
```

#### 基础

##### 运行第一个Chisel构建

- **以GCD.scala为例熟悉Chisel源代码**

进到`src/main/scala/examples/`目录，打开`GCD.scala`文件。它实现了众所周知的GCD算法。

第一个要注意的是`import chisel3._`，它导入了Chisel库文件，这让我们可以把Scala当作硬件构建语言来用。

接下来就是Scala类定义，它就是要实现的Chisel组件，基本上可以认为它对应的就是Verilog里的模块声明。

第二个要注意的是`val io = IO(new Bundle{...})`，它是本组件的I/O规范。

这个bundle有多个参数，这些参数都是它构造的一部分，每个参数都有类型(UInit, Bool等)、位宽，并包装进一个direction(Input 或Output)。

如果不指定位宽，Chisel将推断一个合适的位宽(在本例里默认为1)。

这个`io` Bundle其实是组件接口的构造器。

第三个要注意的是寄存器的声明，它告诉Chisel把`x`和`y`当作类型为UInt()的寄存器。

第四个要注意的是`when`语句，它告诉Chisel条件为真时执行操作。在硬件上，它基本上是一个多路复用器在`when`块里选择值(如果条件为真)，否则就使用默认的赋值或保留寄存器的值。

如果条件为真，赋值将在正时钟边沿执行。

这相当于Verilog里使用`always @ (posedge clk)`来定义同步逻辑。

最后要注意的是输出语句，用于计算`io.out`和`io.valid`。

其实在本例里我们不必指定`x`和`y`的宽度，因为Chisel会基于它们要执行的计算，推断并设置合适的值。

- **使用tester和firrtl解释器进行模拟**

首先，看看Chisel代码`src/test/scala/examples/GCDTests.scala`。它包含了一个`PeekPokeTester`的Chisel实现，和一个tester驱动，这个驱动用来模拟电路、把自己连接到tester、在模拟电路上运行tester。

tester用`poke`来驱动电路输入，用`step`来单周期运行电路，用`expect`来验证它的输出。

然后，在顶层目录下使用要要模拟的电路的名称来执行脚本：

```
./run-examples.sh GCD
```

这将会生成GCD.scala电路的firrtl描述，并使用firrtl解释器使用GCDTests.scala的测试工具来模拟它。最后会输出一些调试信息。

如上脚本还会生成`test_run_dir/examples/GCD/GCD.fir`文件，包含了GCD电路和它的tester的firrtl定义。

- **生成Verilog**

要生成Verilog，要指定后端：

```
./run-examples.sh GCD --backend-name verilator
```

这将指示tester驱动生成Verilog，并使用Verilator生成C++模拟。

执行完毕后将看到一些[success]信息。

在`test_run_dir/examples/GCD/`目录下会看到多了不少文件，需要关注的是：

- GCD.v - Chisel电路的Verilog实现
- GCD.vcd - 模拟期间电路信息的波形输出

Verilog源文件大致可分为三个部分：

- 带有input和output的模块声明
- 用于保存中间值的临时wire和寄存器声明
- 在`always @ (posedge clk)`里的寄存器assignments

可以使用`gtkwave`来看vcd输出。

##### 组合逻辑

- **Scala节点：声明Wires**

用Chisel构建组合逻辑的块是相当简单的；当在Scala里声明一个`val`，它就创建一个代表那个数据的结点。如果该值不是寄存器类型，Chisel编译器会把它当作wire。

因此可以把任意数量的这样的值连接起来，以生成我们想要的值。

假设我们想构造一个完整的加法器。

一个完整的加法器有两个输入`a`和`b`，一个进位输入`cin`，产生一个`sum`和进位输出`cout`。

完整加法器的Chisel代码如下所示：

```scala
class FullAdder extends Module {
  val io = IO(new Bundle {
    val a    = Input(UInt(1.W))
    val b    = Input(UInt(1.W))
    val cin  = Input(UInt(1.W))
    val sum  = Output(UInt(1.W))
    val cout = Output(UInt(1.W))
  })
  // Generate the sum
  val a_xor_b = io.a ^ io.b
  io.sum := a_xor_b ^ io.cin
  // Generate the carry
  val a_and_b = io.a & io.b
  val b_and_cin = io.b & io.cin
  val a_and_cin = io.a & io.cin
  io.cout := a_and_b | b_and_cin | a_and_cin
}
```

`cout`是输入`a`，`b`和`cin`的组合函数。

你会发现为了从`io` bundle访问输入值，你需要先引用`io`，因为输入和输出值都属于`io` bundle。`|`，`&`和`^`操作符分别对应于按位的OR，AND，和XOR操作。

- **位宽接口**

如果没有在Chisel里显式地定义值的宽度，Chisel编译器将基于输入的值推测位宽。

注意在`FullAdder`的定义里，`a_xor_b, a_and_b, b_and_cin, a_and_cin `是没有定义宽度的。

然而，基于输入的计算方式，Chisel将正确推断出那些值都是一个比特的宽度，因为它们的输入都是比特宽度操作的结果。

从生成的Verilog里可见那些值确实是一个比特宽的。

如果我们在`FullAdder`里把宽度改为2比特，那么Chisel编译器会推测中间值`a_xor_b, a_and_b, b_and_cin, a_and_cin`都是两比特宽。从生成的Verilog里可以发现确实如此。

##### 使用寄存器

和Verilog不同，在Chisel里定义一个寄存器实际上是告诉编译器生成一个正边沿触发的寄存器。

在本节中，我们将探索如何通过构造移位寄存器来实例化Chisel中的寄存器。

在Chisel中，当您实例化一个寄存器时，有几种方法可以指定到寄存器的输入的连接。

如GCD所示，您可以“声明”寄存器并在`when...`块里分配它的输入连接到的对象，或您可以简单地将寄存器作为参数的值传递给寄存器。

如果你在构造时使用`next`命名参数或专门的寄存器构造函数`RegNext`，选择将下一个值传递给寄存器，它将无条件地在每个周期中记录下这个新值：

```scala
// 在每个周期里记录新寄存器
val y = io.x
val z = RegNext(y)
```

如果满足某个条件时我们才更新，我们使用`when`块来表明这一点：

```scala
// 当满足条件 a > b 时，才记录新寄存器的值
val x = Reg(UInt())
when (a > b) { x := y }
.elsewhen ( b > a) {x := z}
.otherwise { x := w}
```

需要注意的是，在使用条件方法时，准备分配给寄存器输入的值需要与您声明的寄存器的类型和位宽相匹配。

而在无条件寄存器分配里不需要这么做，因为Chisel会推断类型和位宽。

下面几节将介绍如何使用它们来构造移位寄存器。

- **无条件的寄存器更新**

假定我们想构造4比特的移位寄存器，它有接收一个串口输入`in`并生成一个串口输出`out`。

对于第一个示例，我们将不考虑并行load信号，并假设移位寄存器总是启用的。

我们也不考虑寄存器复位信号。

如果我们实例化，并显式地连接所有的4个寄存器，我们的Chisel代码如下：

```scala
class ShiftRegister extends Module {
  val io = IO(new Bundle {
    val in  = Input(UInt(1.W))
    val out = Output(UInt(1.W))
  })
  val r0 = RegNext(io.in)
  val r1 = RegNext(r0)
  val r2 = RegNext(r1)
  val r3 = RegNext(r2)
  io.out := r3
}
```

从生成的Verilog代码可见，Chisel确实是把我们的设计映射到了移位寄存器。

需要注意的一点是，时钟信号和复位信号都隐式地附加到我们的设计中。

```verilog
module ShiftRegister(input clk, input reset,
    input  io_in,
    output io_out);

  reg[0:0] r3;
  reg[0:0] r2;
  reg[0:0] r1;
  reg[0:0] r0;

  assign io_out = r3;
  always @(posedge clk) begin
    r3 <= r2;
    r2 <= r1;
    r1 <= r0;
    r0 <= io_in;
  end
endmodule
```

- **有条件的寄存器更新**

如前所述，通过使用`when`，`.elsewhen`，`.otherwise`块，Chisel允许你有条件地更新寄存器(使用使能信号)。

假定我们给移位寄存器添加一个使能信号，这将使我们可以依据一个`enable`输入信号，在给定周期里控制是否将数据移入或移出。

新的移位寄存器如下：

```scala
class ShiftRegister extends Module {
  val io = IO(new Bundle {
    val in     = Input(UInt(1.W))
    val enable = Input(Bool())
    val out    = Output(UInt(1.W))
  })

  val r0 = Reg(UInt())
  val r1 = Reg(UInt())
  val r2 = Reg(UInt())
  val r3 = Reg(UInt())

  when (io.enable) {
    r0 := io.in
    r1 := r0
    r2 := r1
    r3 := r2
  }
  io.out := r3
}
```

注意不需要定义`.otherwise`条件，因为Chisel将正确推断出应该保留旧寄存器的值。

- **寄存器复位**

在第一次声明时通过指定一个附加参数，Chisel允许您将同步重置指定给某个值。

在我们的移值寄存器里，让我们添加一个重置功能，它将同步地把所有寄存器的值重置为零。

为了实现这点，我们使用`init`参数为我们的寄存器提供更多的信息，或者带着我们想要同步重置的值使用指定的构造函数：

```scala
class ShiftRegister extends Module {
  val io = IO(new Bundle {
    val in     = Input(UInt(1.W))
    val enable = Input(Bool())
    val out    = Output(UInt(1.W))
  })
  // Register reset to zero
  val r0 = RegInit(0.U(1.W))
  val r1 = RegInit(0.U(1.W))
  val r2 = RegInit(0.U(1.W))
  val r3 = RegInit(0.U(1.W))
  when (io.enable) {
    r0 := io.in
    r1 := r0
    r2 := r1
    r3 := r2
  }
  io.out := r3
}
```

注意要重置的值可以是任何值，只要把零和宽度替换为相应的值即可。

Chisel还有一个隐式的全局`reset`信号，可以在`when`块里使用它。

这个重置信号为了方便就叫`reset`，且不需要声明，但如果要把它作为布尔值，需要加上`toBool` 类型转换(cast)。

加上隐式的全局重置后的移位寄存器：

```scala
class ShiftRegister extends Module {
  val io = IO(new Bundle {
    val in     = Input(UInt(1.W))
    val enable = Input(Bool())
    val out    = Output(UInt(1.W))
  })
  val r0 = Reg(UInt())
  val r1 = Reg(UInt())
  val r2 = Reg(UInt())
  val r3 = Reg(UInt())
  when(reset.toBool) {  // 把reset解释为布尔类型
    r0 := 0.U
    r1 := 0.U
    r2 := 0.U
    r3 := 0.U
  } .elsewhen(io.enable) {
    r0 := io.in
    r1 := r0
    r2 := r1
    r3 := r2
  }
  io.out := r3
}
```

这将生成外观略有不同的Verilog源代码，但仍然与先前实现的移位寄存器有相同的重置功能。

- **时序电路**

可以在`src/main/scala/problems`找到如下的练习。你会发现文件的某一部分已经完成，你需要完成的部分在文件中会指出。练习的答案在`src/main/scala/solutions/`。

第一个问题是写一个时序电路计算`in`值之和。`src/main/scala/problems/Accumulator.scala`是这个电路的截短版本。

`src/test/scala/problems/Accumulator.scala`是一个完整的tester，用于验证你是否正确的设计了这个电路。

运行：

```
./run-problem.sh Accumulator
```

如果你没有完成accumulator它将会报错。

#### 基本类型与操作

#### 实例化模块

#### 编写Scala测试程序

#### 创建你自己的工程

#### 条件赋值与内存

#### Scripting Hardware Generation

### 如何阅读Chisel程序

#### 一个路由器电路的例子

#### 路由器测试器

### 常见问题















