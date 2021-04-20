#### 概述

从C与Rust、Rcore与其它OS对比的角度，描述Rcore。

- Rcore：用Rust编写，运行于qemu和k210。
- Ucore：用C编写，运行于qemu。系统的结构应是最接近Rcore的。
- xv6-riscv：用C编写，运行于qemu。
- RT-Thread：用C编写，运行于k210。

第一节的目标是在屏幕上打印一些字符串。要实现此目标需要做以下的事：

- os成功从CPU手中接管控制权。
- os成功控制uart设备。

#### 从CPU手中接管控制权

##### Rcore

要从CPU手中正确接管管控权，os就需要放在内存中合适的位置，这是由链接脚本控制的。

对于Rcore来说，它的链接脚本是linker-\<platform>.ld。两个平台唯一的差别就是`BASE_ADDRESS`不一样，k210是`0x80020000`，而qemu是`0x80200000`。

硬件会从`0x80000000`开始执行代码，由于Rcore设置了bootloader，所以bootloader会先从`0x80000000`开始执行，再把控制权转交给os。k210只有6M的内存，在bootloader目录下可以查到rustsbi-k210.bin是78K，所以rustsbi-k210设定从它后面128K开始执行os是合适的。qemu默认的内存就达到了128M，rustsbi_qemu.bin是107K，所以rustsbi-qemu设定从它后面2M开始执行os也是没问题的。

##### Ucore

Ucore的链接脚本和Rcore是一样的。

##### xv6-riscv

与Rcore不一样，xv6-riscv的链接脚本设定os是从`0x80000000`开始的，这是因为xv6没有使用bootloader，不用给它留位置。

##### TR-Tread

RT-Thread针对K210的链接脚本是bsp/k210/link.lds，它和xv6-riscv一样，也是从`0x80000000`开始的，因为它没有使用像rustsbi这样的bootloader，而自己直接控制了所有的硬件。

#### 控制uart设备

##### Rcore

os通过K210上的uart设备向主机发送信息，主机接收到信息后打印出来。

Rcore通过宏`println!`打印出"Hello world"。这个宏在src/console.rs里定义，最终调用了`console_putchar()`。`console_putchar()`定义在src/sbi.rs，最终使用`ecall`指令调用了机器态的rustsbi，传递的参数是`SBI_CONSOLE_PUTCHAR`。

上面通过`ecall`进到rustsbi的中断处理入口`_start_trap`，保存寄存器的值然后调用`_start_trap_rust`，即`start_trap_rust`函数。通过`match`运算匹配到`SupervisorEnvCall`，调用到`rustsbi::ecall()`执行具体的处理。从lib.rs可见`ecall()`即`handle_ecall()`，`handle_ecall()`在ecall.rs。在`handle_ecall()`里通过`match`运算匹配到`LEGACY_CONSOLE_PUTCHAR`，进而执行`legacy::console_putchar()`。在ecall/legacy.rs可见`console_putchar()`调用的是`legacy_stdio_putchar()`，从legacy_stdio.rs可见`legacy_stdio_putchar()`的定义，最终使用的crate：`k210_pac`提供的`try_write()`和`try_flush()`两个函数。可以从[docs.rs](docs.rs)网站得到`k210_pac`的详细介绍(包括源码)。

##### Ucore

Ucore也是通过`ecall`指令进到rustsbi，来实现`printf()`函数。从而打印"Hello world"。

##### xv6-riscv

xv6-riscv有自己定义对UART的驱动，详见kernel/uart.c文件。比如，通过`Reg(THR)`即可定位到UART设备的传输保持寄存器(transmit holding register)，只要向这个内存地址写入数据即可实现通过串口输出的功能。

##### RT-Thread

RT-Thread也有定义对K210的驱动，在bsp/k210/driver。比如drv_uart.c即为UART的驱动，其中`_uarths`即为UARTHS寄存器组的起始地址，在`drv_uarths_putc()`函数就实现了向UARTHS的传输保持寄存器`txdata`写入数据的功能。

