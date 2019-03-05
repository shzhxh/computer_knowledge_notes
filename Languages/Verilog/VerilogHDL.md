#### 1. Verilog HDL怎么学？

Verilog HDL只是电路的描述语言，那要学的应该是Verilog描述的各种**基本电路**。要转换思想，先有电路图，再有描述语言。进而联想，C语言应该学的是**标准库**，还有把这些库整合起来的**数据结构和算法**。Java语言应该学的是**面向对象的思想**，以及在面向对象思想的指导下Java的**标准库**、**数据结构和算法**。个人感觉应该在**不会变化且能把基本元素整合在一起**的东西上下功夫，**基本电路**、**标准库**、**基本思想**、**基本数据结构**、**基本算法**等。

#### 2. 基本电路汇总

- **二选一多路选择器**的电路图是怎样的？怎么用Verilog语言描述？
- **3位加法器**的电路图是怎样的？怎么用Verilog语言描述？
- **比较器**的电路图是怎样的？怎么用Verilog语言描述？
- **三态门选择器**的电路图是怎样的？怎么用Verilog语言描述？

#### 3. 什么是综合？

个人理解综合就是把源代码转换为符合硬件规则的代码，一个成功综合的代码就是可以直接烧到FPGA里的。

#### 4. 什么是模块？

模块是Verilog里的基本结构，由两个部分组成：接口描述和逻辑结构描述。感觉接口描述就是黑盒视角，逻辑结构描述就是白盒视角。

#### 5. Verilog里的运算符对应的电路图是什么样的？

这些运算符包括：

- 算术运算符(+,-,*,/,%);
- 赋值运算符(=,<=);
- 关系运算符(>,<,>=,<=);
- 逻辑运算符(&&,||,!);
- 条件运算符(?:);
- 位运算符(~,|,^,&,^~);
- 移位运算符(<<,>>);

#### 6. 什么是不定态？什么是高阻态？

个人认为数字里的一个二进制位就是实际导线上的一个电位(高电平或低电平)。导线上要么是0,要么是1,只有这两种状态。不定态或高阻态都是从和这个数字位相关的电路结构上来说的，如果相关的电路不能确定这个位是0还是1,那就是不定态。如果相关的电路不能控制本导线上的电位，那就是高阻态。

#### 7. 变量对应的电路结构是什么样的？

- wire就是导线连接，用assign关键字获取组合逻辑信号。
- reg是寄存器，需要触发才能改变其值。
- memory是寄存器数组，就是一串连续的寄存器。


