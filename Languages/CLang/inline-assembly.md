官方文档： [Inline-Assembly-HOWTO](http://www.ibiblio.org/gferg/ldp/GCC-Inline-Assembly-HOWTO.html) & [Using-Assembly-Language-with-C](https://gcc.gnu.org/onlinedocs/gcc/Using-Assembly-Language-with-C.html#Using-Assembly-Language-with-C)  

#### 基本内联汇编的格式：
```c
_asm_ _volatile_("instruction list");
```
#### 带有C/C++表达式的内联汇编格式：
```c
_asm_ _volatile_("instruction list":output:input:modify);
// %0,%1代表占位符，从%0到%9共有10个，分别代表第0个到第9个C表达式。
// 匹配限制符从"0"到"9"共10个，也代表的是从0个到9个的C表达式，用于先读后写处理。

// r: auto select a,b,c,d
// q: auto select a,b,c,d
// m: memory
// g: r or m
// a: %eax or %ax or %al
// b: %ebx or %bx or %bl
// c: %ecx or %cx or %cl
// d: %edx or %dx or %dl
// D: %edi or %di
// S: %esi or %si
// i: 立即数
```

#### 一些例子
```c
/*
 * 第1个例子
 */
asm volatile ("inb %1,%0" : "=a" (data) : "d" (port) : "memory");
// data是要输出的c表达式，a是要赋值给data的寄存器，=表示data是只写的（什么都没有表示data是只读的，+表示data是可读写的）
// C表达式port通过寄存器d输入到当前内联汇编语句中
// "=a"(data)为第一个C表达式，其指定的寄存器为a，故%0代表了寄存器a
// "d"(port)为第二个C表达式，其指定寄存器为d,故%1代表了寄存器d
// 内存被修改了，但在输出表达式中未指明"m"约束，故需在modify部分指明"memory"被修改了。
// 如果寄存器在指令列表中被修改了，而输入/输出部分未进行约束，需在modify部分指明这一点。

/*
 * 第2个例子
 */
asm volatile ("outb %0,%1" :: "a" (data), "d" (port));
// 本例中没有输出
// 将data的值通过寄存器a输入，将port的值通过寄存器d输入
// %0代表第一个寄存器a,%1代表第二个寄存器d
// 不存在未被约束过的寄存器或内存，故modify部分无须指定

/*
 * 第3个例子
 */
asm volatile (
    "cld;"
    "repne; insl;"
    : "=D" (addr), "=c" (cnt)		//通过D寄存器输出给addr,通过c寄存器输出给cnt
    : "d" (port), "0" (addr), "1" (cnt)	//port通过d寄存器输入，addr通过D寄存器输入，cnt通过c寄存器输入
    : "memory", "cc");			//cc表示内联汇编语句会影响到eflags寄存器

/*
 * 第4个例子
 */
asm volatile ("btsl %1, %0" :"=m" (* (volatile long * )addr) : "Ir" (nr));
//将 (* addr) 的第 nr 位设为 1。第一个占位符 %0 与 C  语言变量 addr 对应，第二个占位符 %1 与 C 语言变量 nr 对应。
//因此上面的汇编语句代码与下面的伪代码等价：btsl nr, ADDR
//该指令的两个操作数不能全是内存变量，因此将nr的限定字符串指定为“Ir”，将nr与立即数或者寄存器相关联，这样两个操作数中只有addr为内存变量。

/*
 * 第5个例子
 */
asm volatile (
    "int %1;"
    : "=a" (ret)
    : "i" (T_SYSCALL), "0" (SYS_exec), "d" (name), "c" (len), "b" (binary), "D" (size)
    : "memory");
// 执行的是 int T_SYSCALL, 执行前先将SYS_exec传给eax，执行完后将eax的值返回给ret。这实际上是系统调用的指令。
```
