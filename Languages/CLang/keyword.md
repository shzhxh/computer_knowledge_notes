描述c语言中的关键字

#### attribute

官方文档：https://gcc.gnu.org/onlinedocs/gcc/Attribute-Syntax.html

* align属性：内存内按字节对齐
* always_inline属性：GCC会根据代码逻辑判断是否使用inline代码，但如果使用了always_inline,则一定会将函数内嵌进去。
* constructor:在main函数运行之前就可运行完毕。
* destructor:在进程exit之前执行。
* fastcall & regparm:函数传递参数通常是用堆栈，为了加快代码运行速度，使用这两个属性，会用寄存器来传递参数。
* packed属性：按实际大小来计算struct,union,enum数据结构。
* section属性：把代码放到特定的section中。

#### extern

参考自：《The C programming language》1.10外部变量与作用域

作用： 声明外部变量

#### inline

官方文档：https://gcc.gnu.org/onlinedocs/gcc/Inline.html#Inline

作用：表示此函数被调用时将直接插入，而不是像普通函数那样生成调用代码。

#### static

* static全局变量： 
  和其他全局变量一样，都是存储在.data段（已初始化）或者.bss段（未初始化），但它只在定义它的源文件内有效，其他源文件无法访问它。
* static局部变量： 
  static局部变量分配在.data段中（普通局部变量分配在栈空间中）;只能在其作用域中被访问，而不能被其他函数或源文件访问；其值如果没有被用户初始化，则会被系统初始化为0.
* static函数： 
  相当于面向对象语言中的private函数，只在定义他的源文件内有效，其他源文件无法访问它。

#### volatile

参考：[volatile对象](https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html#Volatiles) 
描述：volatile的意思是易变的，此关键字的意思是告诉编译器不要对其所修饰的变量进行优化。

#### continue

* 用于循环体中，continue语句会跳过循环体后续的代码，再次进入循环体继续执行。
* 而break语句会直接跳出循环体，不再执行循环体。另外，在条件判断时，break语句也用于跳出条件判断的语句块。