### JOS各实验的内容
- lab1 Booting a PC
  - The boot loader
  - The kernel
- lab2 Memory Management
  - Physical Page Management
  - Virtual Memory
  - Kernel Address Space
- lab3 User Environments
  - User Environments and Exception Handling
  - Page faults, Breakpoints exceptions and System calls
- lab4 Preemptive Multitasking
  - Multiprocessor Support and Cooperative Multitasking
  - Copy-on-Write Fork
  - Preemptive Multitasking and Inter-Process communication (IPC)
- lab5 File system, Spawn and Shell
  - File system preliminaries
  - The File System
  - Spawning Processes
  - The keyboard interface
  - The Shell
- lab6 Network Driver
- Final JOS project


### JOS的逻辑结构
- bootloader:boot文件夹
- filesystem:fs文件夹
- include:inc文件夹
- kernel:kern文件夹
  - 控制台：console.c
  - 物理内存管理，二级映射：pmap.c
  - 环境：env.c
  - 中断：trap.c
  - 多处理机：mpconfig.c,lapic.c
  - 中断控制器：picirq.c
  - 计数器：time.c
  - PCI设备:pci.c
  - 文件管理
  - 网络管理
  - 用户
  - 进程调度
- 库：lib文件夹
- 网络：net文件夹
- 用户：user文件夹

### JOS实现步骤：(未完成)
  1. 显示字符的bootloader
  2. 将内核载入内存的bootloader
  3. 显示字符的内核
  4. 管理中断、键盘和时钟
    5. 分页机制
