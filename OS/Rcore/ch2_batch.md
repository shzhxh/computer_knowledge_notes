#### 概述

本章的目标是让用户态的程序可以批量执行。其核心在于中断管理系统的实现。

#### crate的结构

crate顶层引入了两处汇编，包含了八个模块，要实现的功能在`rust_main()`函数。

从`linker-<platform>.ld`文件可见，crate的入口为`_start`。`_start`就是通过`global_asm!`宏把`entry.asm`引入实现的。`entry.asm`为rust程序的运行提供必要的准备，即开辟了64K大小的栈。接下来就是进入rust代码从`rust_main`开始执行。

`rust_main`函数首先执行`clear_bss()`清空os的`.bss`段，对于应用程序，它所运行的操作系统会做好这件事，然而本crate自己就是操作系统，所以只能自己来做。接下来执行`trap::init()`把中断处理例程的入口`__alltraps`写入`stvec`寄存器，这样就能处理用户态传过来的中断了。

`batch::init`和`batch::run_next_app`用于批处理。

模块`console`提供了串口输出的功能，即`println!`宏。

模块`lang_items`提供了`panic`函数。

模块`sbi`提供了调用`rustsbi`的方法，本章用到了它的`console_putchar()`和`shutdown()`。

模块`syscall`提供了两个系统调用的支持，`write`和`exit`。

模块`trap`提供了对中断系统的支持。

模块`match`提供了对批处理的支持。

##### C代码的结构

Rust组织内部的模块使用`mod`关键字，使用外部的模块使用`use`关键字。而C不管是使用内部的库还是外部的库，一律使用`#include`宏。

#### 中断系统的实现

当CPU接收到中断请求后，会把`stvec`寄存器里保存的中断例程入口加载到`pc`寄存器，这样就可以通过中断例程来控制中断行为了。

Rcore的中断例程入口是`__alltraps`，它在`trap.S`里通过宏`global_asm!`引入到模块`trap`中。`__alltraps`首先保存进入中断之前的上下文。然后调用`traphandler`用Rust语言来处理中断。返回后从`__restore`开始执行，恢复到中断之前的上下文，最后使用`sret`指令返回到被中断的代码处继续执行。

当CPU在用户态运行的时候，`sscratch`寄存器保存的是内核态时`sp`寄存器的值，这是由`rust_main()`所调用的`batch::run_next_app()`实现的。当`run_next_app()`执行的时候，它会调用`__restore`把内核栈的指针保存在`sscratch`寄存器里。所以，当CPU从用户态进到内核态后，`__alltraps`首先做的事就是交换`sp`和`sscratch`的值，这样`sp`寄存器就指向了内核栈，`sscratch`寄存器就保存了用户栈。

接下来就把从`x1`到`x31`共31个用户态的寄存器保存到栈里，然后再把`sstatus`和`sepc`的值也保存到栈里，因为这两个寄存器的值`trap_handler()`函数要用。然后调用`trap_handler()`进行具体的中断处理。

`trap_handler()`接收的参数是一个对栈桢的引用`&mut TrapContext`，返回的参数也是对这个栈桢的引用，因`trap_handler`有可能需要修改栈桢里某些元素的值。`trap_handler`使用了外部crate - riscv的一些模块，详见[crate doc of riscv](https://docs.rs/riscv/0.6.0/riscv/)。

`scause.cause()`方法把`scause`转换为`scause::Exception`或`scause::Interrupt`。通过`match`运算可见，当前只处理了四种异常，即系统调用、StoreFault、StorePageFault、IllegalInstruction，对于其它的中断则直接执行`panic!`宏。如果是系统调用，则调用`syscall()`函数。如果是写内存引起的异常或指令异常，则输出一条信息，并调用`run_next_app()`。其它情况下，都是执行`panic!`宏。

#### rustsbi对中断的支持

#### 批处理的实现

`APP_MANAGER`是`AppManager`的实例，`AppManager`是对`AppManagerInner`的可变借用，`AppManagerInner`里有三个元素，`num_app`是要管理的APP的数量，`current_app`是当前管理的APP的编号，`app_start`。

`KERNEL_STACK`