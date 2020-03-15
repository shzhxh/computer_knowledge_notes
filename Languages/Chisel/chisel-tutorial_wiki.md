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

##### Chisel赋值与重新赋值

第一次赋值使用`=`，随后每次赋值都必须使用`:=`。

由于我们是在构造数字电路，重新赋值的概念没有多大意义，因为电路结点之间的连接仅需定义一次。但是，有时还是需要重新赋值的，因为Chisel不像Verilog，它是按顺序编译的。所以，当一个值或连接还不确定的时候(会在随后确定)，有必要执行重新赋值。

重新赋值的一个简单例子是构建模块的顶层I/O，在声明的时候输出的值不是立即确定的。

考虑之前的FullAdder电路，对于输出值`io.sum`和`io.cout`就使用了重新赋值，因为我们只有在随后的代码里才能知道它们的值，而在构建io Bundle的时候是不知道它们的值的。本例中其它的值都使用了赋值操作符`=`，因为它们需要被创建。

通常，经验法则是如果一个值已经被赋值则则需要使用重新赋值，否则就应使用赋值符号。注意，如果错误地使用了`=`或`:=`在编译的时候会提示错误。

- **UInt类型**

UInit类型是无符号的整型。对于许多基本运算来说UInt就足够了。如下例子显示了常用的UInt操作

```scala
// 本例中忽略了溢出和下溢
class BasicALU extends Module {
  val io = IO(new Bundle {
    val a = Input(UInt(4.W))
    val b = Input(UInt(4.W))
    val opcode = Input(UInt(4.W))
    val out = Output(UInt(4.W))
  })
  io.out := 0.U //THIS SEEMS LIKE A HACK/BUG
  when (io.opcode === 0.U) {
    io.out := io.a //pass A
  } .elsewhen (io.opcode === 1.U) {
    io.out := io.b //pass B
  } .elsewhen (io.opcode === 2.U) {
    io.out := io.a + 1.U //increment A by 1
  } .elsewhen (io.opcode === 3.U) {
    io.out := io.a - 1.U //increment B by 1
  } .elsewhen (io.opcode === 4.U) {
    io.out := io.a + 4.U //increment A by 4
  } .elsewhen (io.opcode === 5.U) {
    io.out := io.a - 4.U //decrement A by 4
  } .elsewhen (io.opcode === 6.U) {
    io.out := io.a + io.b //add A and B
  } .elsewhen (io.opcode === 7.U) {
    io.out := io.a - io.b //subtract B from A
  } .elsewhen (io.opcode === 8.U) {
    io.out := io.a < io.b //set on A less than B
  } .otherwise {
    io.out :=  (io.a === io.b).asUInt //set on A equal to B
  }
}
```

注意在`when`块里有多个对`io.out`的重新赋值，这意味着依据`io.opcode`的值`io.out`可以取多个不同的值。还须注意，为了定义加到我们的操作数的常量，我们必须把它们定义为UInt类型，因为不允许对不同类型的操作数执行UInt操作。

常用的UInt操作如下：

| Operand | Operation        | Output Type |
| ------- | ---------------- | ----------- |
| +       | Add              | UInt        |
| -       | Subtract         | UInt        |
| *       | Multiply         | UInt        |
| /       | UInt Divide      | UInt        |
| %       | Modulo           | UInt        |
| ~       | Bitwise Negation | UInt        |
| ^       | 按位XOR          | UInt        |
| &       | 按位AND          | UInt        |
| \|      | 按位OR           | UInt        |
| ===     | Equal            | Bool        |
| =/=     | Not Equal        | Bool        |
| >       | 大于             | Bool        |
| <       | Less             | Bool        |
| \>=     | Greater or Equal | Bool        |
| <=      | Less or Equal    | Bool        |

- **按位提取(Bit Extraction)**

UInt类型依据允许你基于索引按位提取。给定一个`n`位的`value`，我们可以提取`x`到`y`之间的位(`n>x>y>=0`)：

```scala
val x_to_y = value(x, y)
```

注意，高的索引放在前面。还要注意，UInt里的位是从0开始索引的，所以最高可以提取的位是`n-1`。

如果只想提取单个的比特，只需指定单个的索引值：

```scala
val x_of_value = value(x)	// 提取第x位
```

如下是一个更具体的位提取的例子。在这个例子里，基于位移的值，从一个字里选一个字节，当从字里载入字节的时候这是常见操作：

```scala
class ByteSelector extends Module {
  val io = IO(new Bundle {
    val in     = Input(UInt(32.W))
    val offset = Input(UInt(2.W))
    val out    = Output(UInt(8.W))
  })
  io.out := 0.U(8.W)
  when (io.offset === 0.U(2.W)) {
    io.out := io.in(7,0)
  } .elsewhen (io.offset === 1.U) {
    io.out := io.in(15,8)
  } .elsewhen (io.offset === 2.U) {
    io.out := io.in(23,16)
  } .otherwise {
    io.out := io.in(31,24)
  }
}
```



- **位串接**

Chisel还允许你使用`Cat`把比特轻松连接起来。

假设你有一根数据总线，你想用word A和B来驱动它。

为了把那两个值连接在一起，我们可以这么干：

```scala
val A = UInt(32.W)
val B = UInt(32.W)
val bus = Cat(A, B) // concatenate A and B
```

`Cat`的第一个参数将位于总线的高位，第二参数将处于总线的低位。

在本例中，总线的0~31位对应B，32~63位对应A。

注意：`Cat`在工具包里，而不是核心包里。所以要使用它，要用如下方法导入：

```
import chisel3.util.Cat		# 导入Cat
import chisel3.util._		# 导入所有的工具
```

确保定义一个内部寄存器，并在断言`inc`的时候更新它。

- **LFSR16**

LFSR16的意思是16位线性反馈移位寄存器。

在src/main/scala/problems/LFSR16.scala，填充对应模块。

将位连接、位提取和xor操作符`^`一起使用。

```bash
./run-problem.sh LFSR16	# 修改你的程序直到本脚本可以正常运行
```



- **UInt操作位推断**

注意，一些操作可能有溢出的情况，如加法和乘法。

请看如下的16位乘法器的实现：

```scala
class HiLoMultiplier() extends Module {
  val io = IO(new Bundle {
    val A  = Input(UInt(16.W))
    val B  = Input(UInt(16.W))
    val Hi = Output(UInt(16.W))
    val Lo = Output(UInt(16.W))
  })
  val mult = io.A * io.B
  io.Lo := mult(15, 0)
  io.Hi := mult(31, 16)
}
```

随着设计日趋复杂，将会看到Chisel的位推断是一个非常有用的特性，可以更高效地构造硬件。常见的位推断如下：

| Operation          | Result Bit Width         |
| ------------------ | ------------------------ |
| *Z = X + Y*        | max(Width(X), Width(Y))  |
| *Z = X - Y*        | max(Width(X), Width(Y))  |
| *Z = X \| Y*       | max(Width(X), Width(Y))  |
| *Z = X ^ Y*        | max(Width(X), Width(Y))  |
| *Z = ~X*           | Width(X)                 |
| *Z = Mux(C, X, Y)* | max(Width(X), Width (Y)) |
| Z = X * Y          | Width(X) + Width(Y)      |
| *Z = X << n*       | Width(X) + n             |
| *Z = X >> n*       | Width(X) - n             |
| *Z = Cat(X, Y)*    | Width(X) + Width(Y)      |
| *Z = Fill(n, x)*   | Width(X) + n             |

##### Chisel布尔类型

布尔类型代表逻辑表达式的结果，有两个值`true`或`false`。可用于条件语句如`when`块。

```scala
val change = io.a === io.b // change就是布尔类型
when (change) {            // exec if change is true
  ...
} .otherwise {
  ...
}
```

可以这样实例化布尔值：

```scala
val true_value  = true.B
val false_value = false.B
```

如`BasicALU`所示，为了像UInt类型那样使用Bool类型并把它赋值给一个输出，需要对UInt进行强制类型转化。

##### 类型强制转换

赋值的时候，要赋的值和被赋值的对象要是相同的类型。如果把布尔类型赋值给一个UInt类型将产生错误。执行这种操作的正确方法是使用`asUInt`把布尔型转换为UInt型。

常用的类型转换有：asUInt()、asSInt()、asBool()。

#### 实例化模块

像其它硬件描述语言一样，Chisel允许直接的模块实例化，以使能模块和层次结构。

在Chisel里实例化一个模块类，相当于在Verilog里实例化一个模块。

实例化一个新的模块，需要使用Scala的`new`关键字，并调用`Module`。

我们要确保把它赋给一个值，这样我们就能引用它的输入和输出，这些输入输出都是我们需要连接的。

比如，使用多次复制`FullAdder`模块的方法来构造一个4位的加法器。

详见src/main/scala/examples/Adder4.scala。

在本例里，注意在引用每个模块的I/O时，我们必须先引用包含了I/O端口的`io`。

另外，注意在给模块的I/O赋值的时候使用了重新赋值的符号`:=`。在实例化模块的时候，要确保你连接到了所有的输入和输出的端口。

如果一个端口没有被连接，Chisel编译器可能会优化掉你的部分设计，因为它发现那些未连接的端口是不必要的，并抛出错误或警告。

##### Vec类

`Vec`类允许你创建一个索引化的向量，这个向量可以填充任意表达式，这些表达式可以返回一个chisel数据类型。

`Vec`的语法如下：

```scala
val myVec = Vec(Seq.fill( <number of elements> ) { <data type> })
```

`<number of elements>`指的是向量的长度，`<data type>`指的是向量包含的类的类型。

例如，实例化一个包含10个条目、值的类型为5位UInt的向量：

```scala
val ufix5_vec10 := Vec(Seq.fill(10) { UInt(5.W) })
```

定义一个向量寄存器：

```scala
val reg_vec32 = Reg(Vec(Seq.fill(32){ UInt() }))
```

只需指定索引号，就可以给`Vec`指定的条目赋值。

例如，给向量`reg_vec32`的第一个条目赋值：

```scala
reg_vec32(0) := 0.U
```

如果要访问向量里的一个元素，只要指定它的索引即可。

例如，访问`reg_vec`的第5个元素：

```scala
val reg5 = reg_vec(5)
```

`Vec`类的语法和实例化一个模块向量略有不同。

在定义模块向量的时候，使用`io` bundle来定义向量的类型。

例如，定义一个16个模块的向量，且使用`FullAdder`：

```scala
val FullAdders = 
  Vec(Seq.fill(16){ Module(new FullAdder()).io })
```

注意，在模块名称`FullAdder`之前我们使用单键字`new`。

- **Vec Shift Reg**

一个简单的移位寄存器，文件在src/main/scala/problems/vecShiftRegisterSimple.scala。

其中`out`的值对于`in`来说有四个周期的延迟复制。

##### 参数化

在之前的加法器例子里，我们显示地实例化了四件份`FullAdder`的拷贝，并连接到端口。

但是考虑一下我们要生成一个n位的加法器。

像Verilog一样，Chisel允许传递参数来定义你的某些设计。

对于一个进位传递加法器，我们会想要参数化一些整型值`n`的宽度，如下所示：

```scala
// 带有进位输入和进位输出的n位加法器
class Adder(n: Int) extends Module {
  val io = IO(new Bundle {
    val A    = Input(UInt(n.W))
    val B    = Input(UInt(n.W))
    val Cin  = Input(UInt(1.W))
    val Sum  = Output(UInt(n.W))
    val Cout = Output(UInt(1.W))
  })
  // create a vector of FullAdders
  val FAs = Vec(Seq.fill(n){ Module(new FullAdder()).io })

  // define carry and sum wires
  val carry = Wire(Vec(n+1, UInt(1.W)))
  val sum   = Wire(Vec(n, Bool()))

  // first carry is the top level carry in
  carry(0) := io.Cin

  // wire up the ports of the full adders
  for(i <- 0 until n) {
    FAs(i).a   := io.A(i)
    FAs(i).b   := io.B(i)
    FAs(i).cin := carry(i)
    carry(i+1) := FAs(i).cout
    sum(i)     := FAs(i).sum.toBool()
  }
  io.Sum  := sum.asUInt
  io.Cout := carry(n)
}
```

注意，我们使用布尔值的`Vec`持续追踪输出的和。因为Chisel不支持直接按位赋值。所以在上面的例子里，为了得到n位宽的`sum`，我们使用了n位宽的布尔值的`Vec`并把它强制转换为UInt()。

你会注意到模块在一个Vec类里实例化，这使得我们在给每个`FullAdder`赋值的时候可以迭代。这和Verilog里的生成语句是相似的。

然后，Chisel是可以提供更强大的变体的。

实例化参数化的模块和实例化非参数化的模块基本类似，不一样的地方是我们必须提供参数。

```scala
val adder4 = Module(new Adder(4))	// 实例化4位的加法器
val adder4 = Module(new Adder(4))	// 实例化4位的加法器，且显式地指定参数n
```

当你的模块有多个参数的时候，显式地指定参数是有用的。

假定你有一个参数化的FIFO模块：

```scala
class FIFO(width: Int, depth: Int) extends Module {...}
```

你可以以任意次序显式地指定参数：

```scala
val fifo1 = Module(new FIFO(16, 32))
val fifo2 = Module(new FIFO(width = 16, depth = 32))
val fifo3 = Module(new FIFO(depth = 32, width = 16))
```



##### 内建原语

像其它HDL一样，Chisel提供了一些非常基本的原语。

这些构造是内建到Chisel编译器中的，并且是免费的。

Reg、UInt和Bundle类就是已经涵盖的原语。

与模块实例化不同，原语不用显式地连接它们的io端口就可以使用。

其它有用的原语还有Mem和Vec类。

在这里我们研究一下`Mux`原语的使用。

- **Mux类**

`Mux`原语是双输入多路复用器。

为使用`Mux`，我们先要定义`Mux`类的语法。

对于双输入多路复用器来说，它有三个输入一个输出。

两个输入对应着数据值`A`和`B`，它们可以是任意宽度和数据类型，但必须是同样的宽度和数据类型。

对于第一个参数`select`，它是一个布尔值，决定了选择哪个值来输出。

`select`为`true`则输出`A`，为`false`则输出`B`。

```scala
val out = Mux(select, A, B)
```

注意，如果我们想显式地实现多路复用器，如何使用Mux原语类型抽象出所需的逻辑结构。

- **参数化宽度的加法器**

下一个任务是构造一个加法器，带有参数化的宽度并使用内建的加法操作符`+`。

文件在src/main/scala/problems/Adder.scala。

其中`out`是`w`宽无符号输入`in0`和`in1`的和。

请注意val是如何添加到宽度参数值的，以允许从tester访问宽度，这个宽度作为加法器模块对象的一个字段。

运行`./run-problem.sh Adder`直到电路通过测试。

#### 编写Scala测试程序

Chisel的基于Scala的testbench是针对设计中简单错误的第一道防线。Scala testbench使用几个独特的Chisel构造来执行此操作。为了了解它是如何工作的，我们首先来看一个简单的例子。

##### Scala测试程序的例子

以ByteSelector.scala及其对应的测试工具ByteSelectorTests.scala为例。

在测试工具`ByteSelectorTests`中，我们看到测试部分是用Scala编写的，在一个`Tester`类定义中有一些Chisel结构。被测设备作为参数`c`传递给我们。

在`for`循环中，使用`poke`将`ByteSelector`的每个输入的赋值设置为适当的值。对于这个特殊的示例，我们通过将输入硬编码为某个已知值并检查4个偏移量是否每个都返回适当的字节来测试`ByteSelector`。为了做到这一点，在每次迭代中，我们都会生成适当的模块输入，并告诉仿真将这个值赋给我们测试的设备的输入`c`。

接下来，是电路的步骤。接下来，我们通过调用`step`函数来进一步仿真。这有效地推进了在时序逻辑里对一个时钟周期的仿真。

最后，检查预期的输出。

在本例中，我们检查`ByteSelector`的期望输出。

这定义了模拟的这个特定周期所期望的参考输出。由于我们测试的电路是可综合的，我们期望我们定义的输出出现在任何仿真的演进中。在检查输出是否生成预期的引用输出之后，`expect`函数将记录`true`或`false`。连续的`expect`的结果被提交到一个名为`ok`的`Tester`字段中，`ok`一开始是`true`。`ok`字段的值决定了tester执行的成败。

实际上，`expect`大致是用`peek`来定义的：

```scala
def expect (data: Bits, expected: BigInt) = 
  ok = peek(data) == expected && ok
```

`peek`从DUT获取信号的值。

##### 仿真调试输出

运行`ByteSelector`里的测试程序`./run-examples.sh ByteSelector --is-verbose`。`is-verbose`标志的作用是获取`peeks`和`pokes`的真正顺序。

当我们运行testbench时，我们会注意到每次调用`step`函数时，仿真都会产生调试输出。每一个调用都向`ByteSelector`提供输入和输出的状态，以及参考输出和预期输出之间的检查是否匹配，如下所示:

```
Starting tutorial ByteSelector
[info] [0.006] Elaborating design...
[info] [0.201] Done elaborating.
Total FIRRTL Compile Time: 363.1 ms
Total FIRRTL Compile Time: 56.7 ms
End of dependency graph
Circuit state created
[info] [0.001] SEED 1505836830809
[info] [0.003]   POKE io_in <- 12345678
[info] [0.004]   POKE io_offset <- 0
[info] [0.004] STEP 0 -> 1
[info] [0.006] EXPECT AT 1   io_out got 78 expected 78 PASS
[info] [0.007]   POKE io_in <- 12345678
[info] [0.007]   POKE io_offset <- 1
[info] [0.007] STEP 1 -> 2
[info] [0.009] EXPECT AT 2   io_out got 97 expected 97 PASS
[info] [0.009]   POKE io_in <- 12345678
[info] [0.010]   POKE io_offset <- 2
[info] [0.010] STEP 2 -> 3
[info] [0.012] EXPECT AT 3   io_out got 188 expected 188 PASS
[info] [0.012]   POKE io_in <- 12345678
[info] [0.012]   POKE io_offset <- 3
[info] [0.012] STEP 3 -> 4
[info] [0.014] EXPECT AT 4   io_out got 0 expected 0 PASS
test ByteSelector Success: 4 tests passed in 9 cycles taking 0.028873 seconds
[info] [0.015] RAN 4 CYCLES PASSED
Tutorials passing: 1
```

还请注意，最后有一个pass断言“PASSED”，它对应于测试程序最末尾的`allGood`。

在本例中，我们知道测试通过了，因为allGood断言的结果是“PASSED”。

在失败的情况下，断言将在这里产生一个“FAILED”的输出消息。

##### 一般测试程序

一般来说，scala 测试程序应该有以下大致结构:

- 使用`poke`设置输入
- 使用`step`高级仿真
- 使用`expect`(且/或`peek`)检查期望值
- 重复，直到所有适当的测试用例都得到验证

对于连续模块，我们可能希望将输出定义延迟到适当的时间，因为在仿真中，`step`函数隐式地将时钟提前一个周期。

与Verilog不同，您不需要显式地指定仿真的时间进度;Chisel会帮你处理这些细节。

##### Max2测试程序

为`Max2`电路写一个tester。要填充的tester在src/test/scala/problems/Max2Tests.scala。

使用如下方法生成随机数：

```scala
val in0 = rnd.nextInt(lim) // 返回0~lim-1之间的随机数
```

运行`./run-problem.sh Max2`直到电路通过测试。

##### 测试程序的局限性

Chisel测试程序适用于简单的测试和少量的仿真迭代。

然而，对于较大的测试用例，Chisel testbench很快就会变得更加复杂和缓慢，这仅仅是由于基础结构的低效。

对于这些更大、更复杂的测试用例，我们建议使用c++仿真器或Verilog测试工具，它们运行得更快，可以处理更严格的测试用例。

#### 创建你自己的工程

为了从头开始创建您自己的项目，您将需要创建一个目录、一个Chisel源文件、一个可选的tester和一个build.sbt配置文件。

你应该克隆 [chisel-template](https://github.com/ucb-bar/chisel-template) ，其中包含一个适合作为模板的最小项目。

按照那个仓库里README文件上的说明去做。

#### 条件赋值与内存

##### 有条件的寄存器更新

如本教程前面所示，使用`when`块执行有条件的寄存器更新，它接受一个Bool值或一些布尔表达式来求值。

在本节中，我们将更全面地探讨如何使用这个`when`有条件更新的结构。

如果when块是由它自己使用的，Chisel将假设如果`when`块的条件的值不为真，那么寄存器值就没有更新。

然而，大多数时候，我们不想把自己限制在一个单一的条件。

因此，在Chisel中，我们使用`.elsewhen`和`.otherwise`语句来在多个可能的寄存器更新之间进行选择，如下面的小节所示。

##### `.elsewhen`子句

在指定条件更新时，我们可能需要检查几个条件，这些条件需要按一定的顺序检查。

为了对寄存器进行更新以实现这一点，我们使用`when`…`.elsewhen`结构。

这类似于顺序编程里的`if...else if`控制结构。

与`else if`子句一样，可以将尽可能多的`.elsewhen`语句链接到一个单独的`when`块中。

一般结构就像下面这样：

```scala
when (<condition 1>) {<register update 1>} 
.elsewhen (<condition 2>) {<register update 2>} 
... 
.elsewhen (<condition N>) {<register update N>}
```

其中`<condition 1>`到`<condition N>`代表它们所对应的`<register update>`段的触发条件。

下面是这条语句的一个实现，实现的是一个简单的栈指针。

假定，我们需要一个指针来追踪栈顶的地址。

给定一个将堆栈指针地址减少1个条目的信号pop和一个将堆栈指针地址增加1个条目的信号push，该指针的实现将如下所示:

```scala
class StackPointer(size:Int) extends Module { 
  val io = IO(new Bundle { 
    val push = Input(Bool()) 
    val en   = Input(Bool()) 
    val pop  = Input(Bool()) 
  })
 
  val sp = RegInit(0.U(log2Ceil(size).W)) 
 
  when (io.en && io.push && (sp != (size-1).U)) {
    sp := sp + 1.U 
  } .elsewhen(io.en && io.pop && (sp > 0.U)) { 
    sp := sp - 1.U 
  } 
}
```

注意，在这个实现中，push信号比pop信号具有更高的优先级，因为在`when`块里它出现的更早。

##### `.otherwise`子句

如果`when`块里所有条件都没有触发，可以使用`.otherwise`子句来指定一个默认的寄存器更新。

`.otherwise`子句和`if...else`块里的else子句相似。

`.otherwise`语句必须放在`when`块的最后。

现在完整`when`块的结构如下所示：

```scala
when (<condition 1>) {<register update 1>} 
.elsewhen (<condition 2>) {<register update 2>} 
... 
.elsewhen (<condition N>) {<register update N>} 
.otherwise {<default register update>}
```

在前面的示例中，我们可以添加一个默认语句，该语句将sp赋值给sp的当前值。

```scala
when(io.en && io.push && (sp != (size-1).U)) { 
  sp := sp + 1.U 
} .elsewhen(io.en && io.pop && (sp > 0.U)) { 
  sp := sp - 1.U 
} .otherwise { 
  sp := sp 
}
```

在本例里，为保留sp值进行显式地赋值是多余的，但是它会捕获`.otherwise`语句。

##### `unless`子句

为了完成`when`语句，Chisel还提供了`unless`语句。

`unless`语句是有条件赋值，它仅在条件为假时触发。

`unlsess`语句的一般结构是：

```scala
unless ( <condition> ) { <assignments> }
```

例如，假设我们要对内存内容进行简单的搜索，并找到包含某些数字的地址。

因为我们不知道搜索需要多长时间，所以当搜索结束时，模块将输出一个done信号，在此之前，我们希望继续搜索内存。

这个模块的Chisel代码如下：

```scala
class MemorySearch extends Module {
  val io = IO(new Bundle {
    val target  = Input(UInt(4.W))
    val address = Output(UInt(3.W))
    val en      = Input(Bool())
    val done    = Output(Bool())
  })
  val index  = RegInit(0.U(3.W))
  val list   = Vec(0.U, 4.U, 15.U, 14.U, 2.U, 5.U, 13.U)
  val memVal = list(index)

  val done = (memVal === io.target) || (index === 7.U)

  unless (done) {
    index := index + 1.U
  }
  io.done    := done
  io.address := index
}
```

在本例中，我们将内存的大小限制为8个条目，并使用一个字面值向量来创建只读内存。

注意，如果unless语句看到done信号被断言，那么它将被用来终止迭代。

否则，它将继续增加内存中的索引，直到找到目标中的值或到达内存中的最后一个索引(7)。

##### 组合条件赋值

你也可以使用`when` `.elsewhen` `.otherwise`块来定义可能包含多个值的组合值。

例如，下面的Chisel代码展示了如何实现一个基本的带有四种操作的算术单元:加，减，传递。

在本例中，我们检查操作码以确定执行哪个操作并有条件地分配输出。

```scala
class BasicALU extends Module {
  val io = IO(new Bundle {
    val a      = Input(UInt(4.W))
    val b      = Input(UInt(4.W))
    val opcode = Input(UInt(2.W))
    val output = Output(UInt(4.W))
  })
  io.output := 0.U
  when (io.opcode === 0.U) {
    io.output := io.a + io.b   // ADD
  } .elsewhen (io.opcode === 1.U) {
    io.output := io.b - io.b   // SUB
  } .elsewhen (io.opcode === 2.U) {
    io.output := io.a        // PASS A
  } .otherwise {
    io.output := io.b          // PASS B
  }
}
```

注意，对于更复杂算术单元或组合块，可以很容易地扩展它来检查更多不同的条件。

##### 只读内存

为了在Chisel中实例化只读内存，我们使用常量字面值的向量。

例如，为了实例化一个值为0到3的4条目只读内存，其定义如下:

```scala
val numbers = 
  Vec(0.U, 1.U, 2.U, 3.U)
```

Vec的宽度是最宽参数的宽度。

注意，我们需要在字面值之后指定它的类型。

访问只读内存中的值就是访问Vec中的条目。

例如，要访问内存的第2个条目，我们将使用:

```scala
val entry2 = numbers(2)
```



##### 读写内存

Chisel里包含了一种原始的内存叫做Mem。

使用Mem类可以构造同步或异步读取的多端口内存。

##### 基本的实例化

Mem构造包含了内存大小和组成它的数据类型。

通用声明的结构如下:

```scala
val myMem = Mem(<size>, <type>)
```

其中`<size>`对应的是内存中条目的数量。

例如，如果您想创建一个32位UInt类型的128项内存，您可以使用以下实例化:

```scala
val myMem = Mem(128, UInt(32.W))
```

注意，当在Chisel中构造一个内存时，不能指定内存内容的初始值。

因此，永远不要对Mem类的初始内容做任何假设。

##### 同步VS异步读取

可以指定同步或异步读取行为。

例如，如果我们想要一个32位UInt类型的异步读取的128项内存，我们将使用以下定义:

```scala
val combMem = 
  Mem(128, UInt(32.W))
```

同样，如果我们想要一个32位UInt类型的同步读取的128项内存，我们使用一个SeqMem对象:

```scala
val seqMem = 
  SyncReadMem(128, UInt(32.W))
```



##### 添加写端口

为了在Mem中添加写端口，我们使用`when`块来允许Chisel推断一个写端口。在`when`块中，我们为写事务指定位置和数据。一般来说，添加写端口需要以下定义:

```scala
when (<write condition> ) { 
  <memory name>( <write address> ) := <write data> 
}
```

这里`<write address>`表示要写入的内存中的项号。

还要注意，我们在写入内存时使用了重分配操作符`:=`。

例如，假设我们有一个32位UInt类型的128个条目内存。

如果我们想写一个32位的值dataIn到内存的位置writeAddr，并且写使能信号`wen`是真的，我们的Chisel代码应该是这样的:

```scala
... 
val myMem = Mem(128, UInt(32.W)) 
val wen = io.writeEnable
val writeAddr = io.waddr
val dataIn = io.wdata
when (wen) { 
  myMem(writeAddr) := dataIn 
} 
...
```



##### 添加读端口

根据指定的读取行为类型，Chisel中为Mem添加读取端口的语法略有不同，可分为异步读取和同步读取内存。

- **异步读端口**

对于异步读内存，将读端口添加到内存中就相当于将一个赋值放在一个具有某些触发条件的`when`块中。

如果希望Chisel推断多个读端口，只需在`when`定义中添加更多的赋值即可。

读端口的一般定义如下:

```scala
when (<read condition>) { 
  <read data 1> := <memory name>( <read address 1> ) 
  ... 
  <read data N> := <memory name>( <read address N>) 
}
```

例如，如果你想要一个32位UInt值的128项的内存，这个内存带有两个异步读端口，这个内存还带有一些读使能`re`和读地址`raddr1`、`raddr2`，我们将使用以下`when`块来定义:

```scala
... 
val myMem = Mem(128, UInt(32.W)) 
val raddr1 = io.raddr
val raddr2 = io.raddr + 4.U
val re = io.readEnable
val read_port1 = UInt(32.W)
val read_port2 = UInt(32.W)
when (re) {
  read_port1 := myMem(raddr1)
  read_port2 := myMem(raddr2)
}
...
```

注意，`read_port1`和`read_port2`的类型和宽度应该与Mem中的条目的类型和宽度匹配。

- **同步读取**

为了向Chisel Mem类添加同步读取端口，Chisel需要将内存的输出分配给一个Reg类型。

与异步读端口一样，同步读分配必须在`when`块中发生。

同步读端口定义的一般结构如下:

```scala
... 
val myMem = SyncReadMem(128, UInt(32.W)) 
val raddr = io.raddr
val read_port = Reg(UInt(32.W)) 
when (re) { 
  read_port := myMem(raddr) 
} 
...
```



##### Action里内存的实例

这里我们提供了一个通过实现堆栈来使用内存的小示例。

假设我们想要实现一个栈，它接受两个信号push和pop，其中push告诉栈将一个输入dataIn推送到栈顶，而pop告诉栈从栈顶取出一个值。

此外，如果未断言，使能信号en将禁用pushing或popping。

最后，堆栈应该总是输出堆栈的顶部值。

```scala
class Stack(size: Int) extends Module {
  val io = IO(new Bundle {
    val dataIn  = Input(UInt(32.W))
    val dataOut = Output(UInt(32.W))
    val push    = Input(Bool())
    val pop     = Input(Bool())
    val en      = Input(Bool())
  })

  // declare the memory for the stack
  val stack_mem = Mem(size, UInt(32.W))
  val sp = RegInit(0.U(log2Ceil(size).W))
  val dataOut = RegInit(0.U(32.W))

  // Push condition - make sure stack isn't full
  when(io.en && io.push && (sp != (size-1).U)) {
    stack_mem(sp + 1.U) := io.dataIn
    sp := sp + 1.U
  }
    // Pop condition - make sure the stack isn't empty
    .elsewhen(io.en && io.pop && (sp > 0.U)) {
    sp := sp - 1.U
  }

  when(io.en) {
    dataOut := stack_mem(sp)
  }

  io.dataOut := dataOut
}
```

由于模块被参数化为具有大小的条目，为了正确提取堆栈指针sp的最小宽度，我们使用log2Ceil(size)。

以2为底的对数取整。

##### Load/Search的内存问题

在这个作业中，写一个内存模块，支持加载元素和搜索，基于以下模板:

```scala
class DynamicMemorySearch(val n: Int, val w: Int) extends Module {
  val io = IO(new Bundle {
    val isWr   = Input(Bool())
    val wrAddr = Input(UInt(log2Ceil(n).W))
    val data   = Input(UInt(w.W))
    val en     = Input(Bool())
    val target = Output(UInt(log2Ceil(n).W))
    val done   = Output(Bool())
  })
  val index  = RegInit(0.U(log2Ceil(n).W))
  val memVal = 0.U
  /// fill in here
  io.done   := false.B
  io.target := index
}
```

注意它如何支持大小和宽度参数n和w，以及如何从大小计算地址宽度。

运行`./run-problems DynamicMemorySearch`直到此电路通过测试。

#### 硬件生成中的脚本化

##### 使用For循环

通常，参数化需要实例化多个组件，这些组件以非常规则的结构连接在一起。

重新回顾参数化的`Adder`组件定义，可以看到`for`循环结构的作用:

```scala
// A n-bit adder with carry in and carry out
class Adder(n: Int) extends Module {
  val io = IO(new Bundle {
    val A    = Input(UInt(n.W))
    val B    = Input(UInt(n.W))
    val Cin  = Input(UInt(1.W))
    val Sum  = Output(UInt(n.W))
    val Cout = Output(UInt(1.W))
  })
  // create a vector of FullAdders
  val FAs = Vec(Seq.fill(n) { Module(new FullAdder()).io })
  val carry = Vec(Seq.fill(n + 1) { UInt(1.W) })
  val sum = Vec(Seq.fill(n) { Bool() })

  // first carry is the top level carry in
  carry(0) := io.Cin

  // wire up the ports of the full adders
  for (i <- 0 until n) {
    FAs(i).a := io.A(i)
    FAs(i).b := io.B(i)
    FAs(i).cin := carry(i)
    carry(i + 1) := FAs(i).cout
    sum(i) := FAs(i).sum.toBool()
  }
  io.Sum := sum.asUInt
  io.Cout := carry(n)
}
```

注意，在`for`循环定义中使用了一个Scala整数`i`值作为索引变量。

这个索引变量被指定为取0 `until` n的值，这意味着它取0、1、2…,n - 1。

如果我们想让它的取值范围从0到n(包括n)，我们可以用`for (i <- 0 to n)`。

还需要注意的是，索引变量`i`实际上并没有在生成的硬件中显示出来。

它完全属于Scala，仅用于声明在Chisel组件定义中如何指定连接。

对于为任意长的`Vec`赋值，for循环结构也非常有用的。

##### 使用If, Else If, Else

之前也提到过，`if, elseif, else`关键字被Scala保留用于控制结构。

这意思着在Chisel里允许您根据提供的参数有选择地生成不同的结构。

当您希望“打开”或“关闭”实现的某些特性，或者希望使用某个组件的不同变体时，这一点特别有用。

例如，假设我们有几个简单的计数器，希望将它们打包成一个通用的计数器模块:UpCounter、DownCounter和OneHotCounter。

从下面的定义中，我们注意到对于这些简单的计数器，I/O接口和参数是相同的:

```scala
// Simple up counter that increments from 0 and wraps around
class UpCounter(CounterWidth: Int) extends Module {
  val io = IO(new Bundle {
    val output = Output(UInt(CounterWidth.W))
    val ce     = Input(Bool())
  })...
}

// Simple down counter that decrements from
// 2^CounterWidth-1 then wraps around
class DownCounter(CounterWidth: Int) extends Module {
  val io = IO(new Bundle {
    val output = Output(UInt(CounterWidth.W))
    val ce     = Input(Bool())
  })...
}

// Simple one hot counter that increments from one hot 0 
// to CounterWidth-1 then wraps around
class OneHotCounter(CounterWidth:Int) extends Module {
  val io = IO(new Bundle {
    val output = Output(UInt(CounterWidth.W))
    val ce     = Input(Bool())
  })...
}
```

我们可以实例化所有这三个计数器并在它们之间进行多路复用，但是如果我们在任何时候都只需要一个计数器，这将是对硬件的浪费。

为了在这三个要实例化的计数器之间进行选择，我们可以使用Scala的`if, lese if, else`语句，来告诉Chisel如何根据`CounterType`参数选择要实例化的组件:

```scala
class Counter(CounterWidth: Int, CounterType: String) 
    extends Module {
  val io = IO(new Bundle {
    val output = Output(UInt(CounterWidth.W))
    val ce     = Input(Bool())
  })
  if (CounterType == "UpCounter") {
     val upcounter = new UpCounter(CounterWidth)
     upcounter.io.ce := io.ce
     io.output := upcounter.io.output
  } else if (CounterType == "DownCounter") {
    val downcounter = new DownCounter(CounterWidth)
    downcounter.io.ce := io.ce
    io.output := downcounter.io.output
  } else if (CounterType == "OneHotCounter") {
    val onehotcounter = new OneHotCounter(CounterWidth)
    onehotcounter.io.ce := io.ce
    io.output := onehotcounter.io.output
  } else {
    // default output 1
    io.output := 1.U
  }
}
```

通过将这三个计数器组件合并到一个`Counter`模块中，我们可以通过简单地更改参数`CounterType`来实例化一个不同的计数器。

例如:

```scala
// instantiate a down counter of width 16
val downcounter = 
  Module(new Counter(16, "DownCounter"))

// instantiate an up counter of width 16
val upcounter = 
  Module(new Counter(16, "UpCounter"))

// instantiate a one hot counter of width 16
val onehotcounter = 
  Module(new Counter(16, "OneHotCounter"))
```

这允许它们之间的无缝切换。

##### 使用Def

Chisel还允许使用Scala `def`语句来定义可能经常使用的Chisel代码。

这些`def`语句可以封装到Scala对象中，然后在模块中调用。

下面的Chisel代码显示了使用`def`的计数器的替代实现，`def`在`inc`信号被断言时递增`amt`。

```scala
object Counter {
  def wrapAround(n: UInt, max: UInt) =
    Mux(n > max, 0.U, n)

  def counter(max: UInt, en: Bool, amt: UInt) = {
    val x = RegInit(0.U(max.getWidth.W))
    x := wrapAround(x + amt, max)
    x
  }
}

class Counter extends Module {
  val io = IO(new Bundle {
    val inc = Input(Bool())
    val amt = Input(UInt(4.W))
    val tot = Output(UInt(8.W))
  })
  io.tot := counter(255.U, io.inc, io.amt)
}
```

在本例中，我们调用定义在`Counter`对象中的子例程来执行适当的逻辑。

##### 参数化的Vec移位寄存器

下一个任务是构造一个带延迟参数的移位寄存器。

文件在`src/main/scala/problems/VecShiftRegisterParam.scala`。

其中`out`是在`in`上的对值的`n`周期延迟的拷贝。

还需要注意`val`是如何添加到每个参数值以允许那些值可以从tester那里访问的。

修改完成后运行`./run-problem.sh VecShiftRegisterParam`直到电路通过测试。

##### Mul查找表

下一个作业是用`Vec`写一个16x16的乘法表。

文件在`src/main/scala/problems/Mul.scala`。

作为一个构建查找表的提示：使用从`tab`查找表构建的rom，这个`tab`查找表可以看作是一个带着增量添加元素的Scala ArrayBuffer(使用`+=`):

```scala
val tab = Vec(muls)
```

然后使用由`x`和`y`输入组成的地址查找结果，如下所示:

```scala
io.z := tab(Cat(io.x, io.y))
```

运行`./run-problem.sh Mul`直到电路通过测试。

### 如何阅读Chisel程序

#### 一个路由器电路的例子

#### 路由器测试器

### 常见问题















