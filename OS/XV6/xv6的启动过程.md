#### x86架构

bootblock->entry.S->main.c

##### 1. 从MBR开始执行

从bootasm.S的`start`标记处开始，关中断，开启A20地址线，开启保护模式，设置段选择子，调用bootmain函数。

`bootmain`函数在bootmain.c，其作用是将内核载入内存。然后跳转到内核的入口开始执行，内核的入口为`_start`。

##### 2. 内核的入口_start

`_start`在entry.S，开启分页，然后跳转到main函数开始执行。

##### 3. main函数

`main`函数在main.c，是内核的主控函数。

##### 4. kinit1

物理页分配

在`kalloc.c`里实现kinit1函数(不需要锁的物理页分配)，其原理是把前4M的物理页放到一个单向链表里。kinit1和kinit2调用freerange将内存加入空闲链表中, freerange则是通过对每一页调用kfree实现该功能。一个PTE只能指向一个4096字节对齐的物理地址(即是4096的倍数),因此freerange用PGROUNDUP来保证分配器只会释放对齐的物理地址。分配器原本一开始没有内存可用,正是对kfree的调用将可用内存交给了分配器来管理。

##### 5. kvmalloc

内核页表

在`vm.c`里实现kvmalloc函数(建立内核的页表映射)。

##### 6. mpinit

侦测其它的处理器。

##### 7. lapicinit

初始化LAPIC中断控制器。时钟芯片是在LAPIC中的，所以每一个处理器可以独立地接收时钟中断。

`lapicw(TIMER, PERIODIC | (T_IRQ0 + IRQ_TIMER));`使LAPIC周期性地在IRQ_TIMER(也就是IRQ0)产生中断。

`lapicw(TPR, 0);`打开CPU的LAPIC的中断,这使得LAPIC能够将中断传递给本地处理器。

##### 8. seginit

初始化段描述符(在不同CPU上有不同值的per-CPU变量)。

##### 9. picinit

使PIC失效。

```
void picinit(void){
  outb(IO_PIC1+1, 0xFF);
  outb(IO_PIC2+1, 0xFF);
}
```

##### 10. ioapicinit

ioapicinit是对I/O APIC的初始化。它建立了中断到PRT的映射，然后把它们都屏蔽掉。随后由设备自己开启中断，并指定哪个处理器接收这个中断。

```
for(i = 0; i <= maxintr; i++){
    ioapicwrite(REG_TABLE+2*i, INT_DISABLED | (T_IRQ0 + i));
    ioapicwrite(REG_TABLE+2*i+1, 0);
  }
```

##### 11. consoleinit

初始化硬件：控制台。

##### 12. uartinit

初始化硬件：串口。

##### 13. pinit

初始化进程表。

##### 14. tvinit

在`trap.c`里实现`tvinit`函数(建立中断向量表)。`tvinit`函数通过宏SETGATE建立了中断向量表的256个表项，详见下表：

| SETGATE参数 | 值           | 意义                       |
| ----------- | ------------ | -------------------------- |
| gate        | idt[i]       | 中断向量表第i项            |
| istrap      | 0            | 中断类型，0：中断，1：陷阱 |
| sel         | SEG_KCODE<<3 | 段选择子                   |
| off         | vectors[i]   | 第i个中断例程的入口地址    |
| d           | 0            | 描述符特权级               |

通过观察`vectors.S`可以发现所有的中断例程都指向了alltraps，所以个人认为在xv6里中断向量表并没有发挥它应有的作用，因为所有的中断都是在alltraps及其后的函数里处理的。中断向量表里每项默认的值：args是双字计数，只有在调用门中有意义，默认为0；rsvl是保留位，默认为0；s位固定为0；p描述符是否有效，默认为1。

特别地，`tvinit`函数为系统调用T_SYSCALL进行了专门的设置：将istrap设置为1将它指定为一个陷阱门，陷阱门不会清除EFLAGS寄存器的IF位，这使得处理系统调用的时候也可以处理其它中断；将d设置为DPL_USER，这使得用户进程可以通过int指令产生一个内陷，因为xv6不允许进程用int来产生其它中断，否则会触发13号中断(通用保护异常)。

##### 15. binit

binit 缓冲区缓存。binit会从一个静态数组buf中构建出一个有NBUF个元素的双向链表。所有对块缓冲的访问都通过链表而非静态数组。

##### 16. fileinit

初始化文件表。

##### 17. ideinit

初始化磁盘。首先，调用`ioapicenable`来打开最后一个CPU(ncpu-1)的IRQ_IDE中断。（在双处理器系统上，CPU1专门处理磁盘中断。）然后，调用`idewait`来等待磁盘接受命令。`idewait`通过0x1F7端口获取磁盘状态，直到IDE_BSY被清除以及IDE_DRDY被设置。最后，检查磁盘1是否存在。初始化完成后，xv6只能通过buffer cache调用`iderw`读写磁盘了。

##### 18. startothers

启动其它处理器。

##### 19. kinit2

使用了锁的物理页分配，使更多的内存可用于物理页分配。

##### 20. userinit

初始化第一个用户进程。

##### 21. mpmain

完成此处理器的设置，开启多处理器切换和进程调度。

`idtinit`载入idt寄存器。

#### riscv架构

##### 1. 入口在_entry

文件位置：kernel/entry.S，目的是为c代码的运行指定栈的位置，为每个核分配了4096字节的栈空间。一旦分配完成，跳转到start函数。

##### 2. start函数

文件位置：kernel/start.c，目的是为进入S特权级做好准备，一旦准备完成就进入S态，从main函数开始执行。

1. 首先将mstatus寄存器的MPP置为1，1代表了S特权级，这样执行mret指令的时候将返回到S态。
2. 将mepc寄存器写入main函数的地址，这样执行mret指令后会从main函数开始执行。
3. 将satp寄存器写入0，这样就不会开启分页机制，而直接使用的物理地址。
4. 将medeleg和mideleg寄存器写入0xffff，因为medeleg的低16位是有意义的(对应着16个异常)，mideleg寄存器的低12位是有意义的，那么这样把两个寄存器的低16位都写入1相当于是把所有的异常和中断的处理都授权给S态了。
5. 设置计时器中断。**TODO**:为什么在M态设置计时器中断？
6. 将寄存器mhartid的值写入到寄存器tp。
7. 执行mret指令，进入S特权级，执行main函数。

##### 3. main函数

文件位置：kernel/main.c，这是整个内核的总控函数。如果当前的核编号为0，则为主控核，执行一系列的硬件初始化工作，然后执行scheduler函数进入处理器调度。如果当前的核编号不为0，则初始化当前核的数据结构，然后执行scheduler函数进入处理器调度。

##### 4. consoleinit函数

文件位置：kernel/console.c，其作用是初始化控制台，它是printf函数的基础。

首先初始化cons.lock。cons是控制台的数据结构，它包含了一个spinlock，一个128字节的buf，read、write、edit索引各一个。

然后初始化uart。**TODO**：请解释uart的寄存器用法。

最后设置devsw[CONSOLE]的read和write操作。consoleread和consolewrite实现对控制台的读和写，它们最终都是通过调用uart来实现其功能的。devsw[]是个数组，每一项都代表一个设备，比如devsw[CONSOLE]就代表了控制台，每项都是个数据结构，包含了read和write两种操作。

##### 5. printfinit函数

文件位置：kernel/printf.c，其作用是避免多个核调用printf函数引起输入输出的混乱，它是用自旋锁来实现这一功能的。

这个函数所做的就是初始化printf函数的自旋锁。pr就是printf函数使用自旋锁的数据结构，它包含了一个自旋锁lock和自旋锁的状态标志locking。

##### 6. printf函数

文件位置：kernel/printf.c，向屏幕输出带有格式的字符串。printf依赖于consputc，consputc依赖于uartputc。

##### 7. kinit函数

文件位置：kernel/kalloc.c，分配物理内存。

首先，初始化物理内存的自旋锁。kmem是物理内存的数据结构，它包含了一个自旋锁lock和一个单向链表freelist。

然后，执行freerange函数，把内核之后直到128M的内存空间放到单向链表里，这样就可以通过链表来管理这些空闲内存了。

##### 8. kvminit函数

文件位置：kernel/vm.c，创建内核页表。

- 使用kalloc函数分配一个空闲页，这个空闲页分配给标记kernel_pagetable，这个标记代表了内核的页表。
- 接下来是为uart、virtio硬盘、clint和plic建立了映射，它们的虚拟地址和物理地址是相同的。
- 接下来为内核的代码段和数据段也建立了映射。
- 最后为管理内核/用户空间切换的代码空间建立映射。
- kvmmap函数的作用是给内核的页表添加一个映射，kvmmap又是通过mappages函数实现其功能的。PGROUNDDOWN这个宏的作用是给出某个地址所在的页，所以变量a代表虚拟地址va所在的页。size代表了要映射的内存区域的大小，(va+size-1)代表了要映射的虚拟内存区域的末尾，所以变量last代表要映射的虚拟内存区域最后一部分所在的页。walk函数的作用是找到在映射关系里虚拟地址所对应的物理地址的入口(pte)。接下来就把物理地址存储到这个入口里。

##### 9. kvminithart

文件位置：kernel/vm.c，开启分页。

- 使用w_satp函数向satp寄存器写入内核页表，其采用的是Sv39的分页方式。
- 使用sfence_vma函数刷新TLB，这样才能使分页机制真正生效。

##### 10. procinit

文件位置kernel/proc.c，初始化进程表。

- 初始化进程的自旋锁pid_lock
- xv6设定的最大PCB个数为64，对于每一个PCB都为其分配4K的栈空间，这些栈的虚拟地址是从TRAMPOLINE开始往下排的(TRAMPLINE指向的是虚拟地址空间里最顶端的那个页)，这些虚拟地址都保存在各自PCB的kstack标记处。proc[]是PCB所在的数组，数组的每一个元素都是一个PCB。
- 最后，使用kvminithart函数刷新PLB，使如上虚拟地址到物理地址的映射生效。

##### 11. trapinit

文件位置kernel/trap.c，初始化trap对应的锁。

- 只有一条语句，就是初始化自旋锁tickslock。

##### 12. trapinithart

文件位置kernel/trap.c，安装内核trap vecor。

- 向stvec寄存器写入kernelvec的地址。kernelvec是中断处理例程的入口，在kernel/kernelvec.S，它保存当前寄存器的值，然后执行中断处理函数kerneltrap，从kerneltrap返回后执行sret返回。

##### 13. plicinit

文件位置kernel/plic.c，设置中断控制器。

- 向PLIC的UART0和VIRTIO0中断使能位写入1，使对应的中断生效。

##### 14. plicinithuart

文件位置kernel/plic.c，为设备中断申请PLIC。

- 向PLIC里当前核对应的寄存器写入数据，使能当前核的UART0和VIRTIO0中断，且设置当前核的priority threshold为0。**TODO**：请解释清plic寄存器的用法。

##### 15. binit

文件位置kernel/bio.c，初始化buffer cache。

- bcache是buffer cache的数据结构。首先初始化buffer cache的自旋锁bcache.lock。
- bcache.head是双向链表的结点，用来把数组buf的各个元素链接起来。最终，bcache.head指向了数组的尾部。

##### 16. iinit

文件位置kernel/fs.c，初始化inode cache。

- icache是inode cache的数据结构。它包含了inode cache的自旋锁和数组inode。这个函数初始化自旋锁icache.lock和各个inode的睡眠锁(sleep lock)。

##### 17. fileinit

文件位置kernel/file.c，初始化文件表。

- ftable是文件表的数据结构。它包含了文件表的自旋锁ftable.lock和数组file。这个函数仅仅是初化文件表的自旋锁ftable.lock。

##### 18. virtio_disk_init

文件位置kernel/virtio_disk.c，初始化硬盘。

- **TODO**：请解释清disk寄存器的用法。

##### 19. userinit

第一个用户进程。

- 为用户进程分配资源，并将其状态改为RUNNABLE。当内核开始调度的时候第一个用户进程就开始执行了。

##### 20. __sync_synchronize

gcc内建函数，告诉编译器在这个点之后不要移动内存读写指令(load和store)，这样可以用来确保锁不会被破坏。

##### 21. scheduler

每个核都会运行这个函数，在所有进程里找状态为RUNNABLE的进程然后进行它。

- 数组cpus[]的元素是cpu的数据结构，mcpu()返回的是当前cpu所对应的那个cpus[]里的元素。
- intr_on()使能当前核的中断。
- acquire和release分别用于锁的请求和释放。
- swtch用于切换上下文，“返回”用户态。