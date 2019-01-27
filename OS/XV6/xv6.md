### 参考资料

- [xv6源代码](https://github.com/mit-pdos/xv6-public.git)
- [xv6中文文档](https://www.gitbook.io/book/th0ar/xv6-chinese)

### 文件结构

#### 1. 从编译过程观察

`make -n > make.txt`

- kernel.img
  1. bootasm.S和bootmain.c编译为bootblock
  2. bio.c, console.c, exec.c, file.c, fs.c, ide.c, ioapic.c, kalloc.c, kbd.c, lapic.c, log.c, main.c, mp.c, picirq.c, pipe.c, proc.c, sleeplock.c, spinlock.c, string.c, swtch.S, syscall.c, sysfile.c, sysproc.c, trapasm.S, trap.c, uart.c, vectors.pl, vm.c, entry.Sentryother.S, initcode.S编译为kernel
  3. 将boolblock和kernel装入kernel.img里
- fs.img
  1. 使用ulib.c, usys.S, printf.c, umalloc.c做为用户库。
  2. 使用以上库，将cat.c, echo.c, forktest.c, grep.c, init.c, kill.c, ln.c, ls.c, mkdir.c, rm.c, sh.c, streefs.c, usertests.c, wc.c, zombie.c编译为对应的应用程序。
  3. 使用mkfs将以上应用程序放到一个文件系统里，即为fs.img。

#### 2. 从运行过程观察

- bootblock->entry.S->main.c
- bootblock的执行过程。关中断，开启A20地址线，开启保护模式，设置段选择子，将内核载入内存。
- main函数的执行过程
  1. 在`kalloc.c`里实现kinit1函数(不需要锁的物理页分配)，其原理是把前4M的物理页放到一个单向链表里。kinit1和kinit2调用freerange将内存加入空闲链表中, freerange则是通过对每一页调用kfree实现该功能。一个PTE只能指向一个4096字节对齐的物理地址(即是4096的倍数),因此freerange用PGROUNDUP来保证分配器只会释放对齐的物理地址。分配器原本一开始没有内存可用,正是对kfree的调用将可用内存交给了分配器来管理。

  2. 在`vm.c`里实现kvmalloc函数(建立内核的页表映射)。

  3. mpinit侦测其它的处理器。

  4. lapicinit初始化LAPIC中断控制器。时钟芯片是在LAPIC中的，所以每一个处理器可以独立地接收时钟中断。

     `lapicw(TIMER, PERIODIC | (T_IRQ0 + IRQ_TIMER));`使LAPIC周期性地在IRQ_TIMER(也就是IRQ0)产生中断。

     `lapicw(TPR, 0);`打开CPU的LAPIC的中断,这使得LAPIC能够将中断传递给本地处理器。

  5. seginit初始化段描述符(在不同CPU上有不同值的per-CPU变量)。

  6. picinit使PIC失效。

     ```c
     void picinit(void){
       outb(IO_PIC1+1, 0xFF);
       outb(IO_PIC2+1, 0xFF);
     }
     ```

  7. ioapicinit是对I/O APIC的初始化。它建立了中断到PRT的映射，然后把它们都屏蔽掉。随后由设备自己开启中断，并指定哪个处理器接收这个中断。

     ```c
     for(i = 0; i <= maxintr; i++){
         ioapicwrite(REG_TABLE+2*i, INT_DISABLED | (T_IRQ0 + i));
         ioapicwrite(REG_TABLE+2*i+1, 0);
       }
     ```

  8. consoleinit初始化硬件：控制台。

  9. uartinit初始化硬件：串口。

  10. pinit初始化进程表。

  11. 在`trap.c`里实现`tvinit`函数(建立中断向量表)。`tvinit`函数通过宏SETGATE建立了中断向量表的256个表项，详见下表：

      | SETGATE参数 | 值           | 意义                       |
      | ----------- | ------------ | -------------------------- |
      | gate        | idt[i]       | 中断向量表第i项            |
      | istrap      | 0            | 中断类型，0：中断，1：陷阱 |
      | sel         | SEG_KCODE<<3 | 段选择子                   |
      | off         | vectors[i]   | 第i个中断例程的入口地址    |
      | d           | 0            | 描述符特权级               |

      通过观察`vectors.S`可以发现所有的中断例程都指向了alltraps，所以个人认为在xv6里中断向量表并没有发挥它应有的作用，因为所有的中断都是在alltraps及其后的函数里处理的。中断向量表里每项默认的值：args是双字计数，只有在调用门中有意义，默认为0；rsvl是保留位，默认为0；s位固定为0；p描述符是否有效，默认为1。

      特别地，`tvinit`函数为系统调用T_SYSCALL进行了专门的设置：将istrap设置为1将它指定为一个陷阱门，陷阱门不会清除EFLAGS寄存器的IF位，这使得处理系统调用的时候也可以处理其它中断；将d设置为DPL_USER，这使得用户进程可以通过int指令产生一个内陷，因为xv6不允许进程用int来产生其它中断，否则会触发13号中断(通用保护异常)。

  12. binit 缓冲区缓存。binit会从一个静态数组buf中构建出一个有NBUF个元素的双向链表。所有对块缓冲的访问都通过链表而非静态数组。

  13. fileinit初始化文件表。

  14. ideinit初始化磁盘。首先，调用`ioapicenable`来打开最后一个CPU(ncpu-1)的IRQ_IDE中断。（在双处理器系统上，CPU1专门处理磁盘中断。）然后，调用`idewait`来等待磁盘接受命令。`idewait`通过0x1F7端口获取磁盘状态，直到IDE_BSY被清除以及IDE_DRDY被设置。最后，检查磁盘1是否存在。初始化完成后，xv6只能通过buffer cache调用`iderw`读写磁盘了。

  15. startothers启动其它处理器。

  16. kinit2函数是使用了锁的物理页分配，使更多的内存可用于物理页分配。

  17. userinit 初始化第一个用户进程。

  18. mpmain函数完成此处理器的设置，开启多处理器切换和进程调度。

      `idtinit`载入idt寄存器。

### 逻辑结构

#### 1. 进程管理

- 进程控制块`struct proc`维护了进程的信息，**页表**信息描述了进程的内存分配情况，**内核栈**使进程可以在用户态和内核态间切换，**进程状态**使进程可以在不同状态间切换。
- `proc.c`的userinit函数创建了第一个进程。详见：第一章 第一个进程 **代码：创建第一个进程**。
- `proc.c`的sleep和wakeup是非常普通但有效的同步方法。`sleep`是将当前进程转化为`SLEEPING`状态并调用`sched`以释放CPU，`wakeup`是寻找一个睡眠状态的进程并把它标记为`RUNNABLE`。
- `proc.c`的wait,exit,kill的实现。详见：第五章 **代码:wait,exit,kill**
- `initcode.S`的作用是触发exec系统调用。详见：第一章 第一个进程 **第一个系统调用：exec**。
- `exec.c`的exec函数是创建地址空间中用户部分的系统调用。详见：第二章 页表 **代码：exec**。
- `elf.h`描述了xv6应用程序的格式。

#### 2. 物理内存管理

- `mmu.h`是对物理内存管理底层的定义，包括与页表相关的结构体。

- `entry.S`是内核入口，作用是建立C语言的执行环境。主要是建立页表映射和设置栈。详见： 第一章 第一个进程 **代码：第一个地址空间**。

- `vm.c`里的kvmalloc函数建立新的页表映射，并通过修改cr3寄存器转换到新的映射中。详见：第二章 页表 **代码：建立一个地址空间**。

- 内核地址空间到物理内存的映射

  ![kmemlayout](_img/kmemlayout.png)

- 用户地址空间的结构

  ![umemlayout](_img/umemlayout.png)

#### 3. 中断管理

- xv6	在启动主	cpu和其他cpu时屏蔽中断。然后由每个处理器的调度器打开中断。为了控制一些特殊的代码片段不被中断,xv6在进入这些代码片段之前关中断(例如switchuvm)。

- 中断的过程(用户模式从1开始。内核模式从2开始，且在第8步跳回的是内核空间。)

  1. 从任务段描述符中加载%esp和%ss，把老的%ss和%esp压入新的栈中。

  2. 把%eflags, %cs, %eip压栈。

  3. 对某些内陷，处理器会压入一个错误字。

  4. 处理器从IDT中加载新的%eip和%cs，跳转到某个中断例程。

  5. 大部分中断例程都是压入一个错误码(其实就是0)和中断号，然后跳转到alltraps。少部分中断例程不压入错误码，其错误码是由CPU压入的(8号，10～14号，17号)。

  6. alltraps在`trapasm.S`里，它是将`trapframe`结构体定义的剩余寄存器压入栈中，这个结构体包含了中断发生时处理器的状态。

     ```c
     struct trapframe {
       // registers as pushed by pusha
       uint edi;
       uint esi;
       uint ebp;
       uint oesp;      // useless & ignored
       uint ebx;
       uint edx;
       uint ecx;
       uint eax;

       // rest of trap frame
       ushort gs;
       ushort padding1;
       ushort fs;
       ushort padding2;
       ushort es;
       ushort padding3;
       ushort ds;
       ushort padding4;
       uint trapno;

       // below here defined by x86 hardware
       uint err;
       uint eip;
       ushort cs;
       ushort padding5;
       uint eflags;

       // below here only when crossing rings, such as from user to kernel
       uint esp;
       ushort ss;
       ushort padding6;
     };
     ```

  7. alltraps接着将%esp压栈作为`trap`的参数，调用`trap`函数处理中断，`trap`返回后，alltraps弹出栈上的参数，然后执行trapret的代码。

  8. trapret弹出中断桢以恢复用户模式下的寄存器，然后执行iret跳回用户空间。

- `traps.h`记录了所有确定的中断类型。


- `trap.c`的trap函数是中断的主控函数。它是依据中断号`tf->trapno`来判断自己被调用的原因以及自己该做些什么的。首先，它会判断调用原因是否是系统调用，如果是则调用系统调用处理程序`syscall`。然后，它会判断调用原因是否是硬件中断，如果是硬件中断则执行相应的中断函数。最后，如果它即不是系统调用也不是硬件中断，trap就认为它是一个异常。如果是内核错误，则打印错误细节并调用`panic`，如果是用户程序的错误，则打印错误细节并设置`myproc()->killed`使之在trap函数接下来的部分被清除掉。

  当因时钟中断而调用trap时, trap只完成两个任务:递增时钟变量的值,并且调用wakeup。

  ```c
    case T_IRQ0 + IRQ_TIMER:
      if(cpuid() == 0){
        acquire(&tickslock);
        ticks++;
        wakeup(&ticks);
        release(&tickslock);
      }
      lapiceoi();
      break;
  ```

  ​

- `syscall.c`的syscall函数用于处理系统调用。首先，它从中断桢中读出系统调用号，系统调用号保存在%eax中(详见`usys.S`)，是系统调用表(syscalls[])的索引。然后，当系统调用表中的系统调用返回时，它的返回值被保存在中断桢的%eax中，这样当系统调用返回用户空间时，这个返回值就从中断桢中加载到寄存器%eax中。

- `syscall.c`的argint, argptr和argstr用于获取第n个系统调用的参数，它们分别用于获得整数，指针和字符串起始地址。`argint`其实就是对`fetchint`的封装，`fetchint`的作用是把一个内存地址中的整型值保存到一个指针里。`argstr`其实是对`fetchstr`的封装，`fetchstr`的作用是字符串的首地址保存在指针里。

- `picirq.c`是早期主板的简单的可编程中断控制器(PIC)的管理代码。

- `ioapic.c`处理的是多处理器I/O系统中的中断，它维护了一个表(PRT,Programmable Redirection Table)，处理器通过MMIO读写这张表而实现中断管理。

- `lapic.c`是关联到每一个处理器上的中断。

- `buf.h`里的`struct buf`是磁盘驱动程序用来表示一个磁盘扇区的。

  ```c
  struct buf {
    int flags;		// 内存和磁盘的关系:B_VALID,数据已被读入。B_DIRTY，数据需要被写出。B_BUSY,一个锁，代表此缓冲区被某个进程锁住了。
    uint dev;			// 设备号
    uint blockno;		// 扇区号
    struct sleeplock lock;
    uint refcnt;
    struct buf *prev; // LRU cache list
    struct buf *next;
    struct buf *qnext; // disk queue
    uchar data[BSIZE];	// 该扇区数据在内存中的拷贝
  };
  ```

- `ide.c`是磁盘驱动。

  `iderw`函数根据标志位更新一个锁住的缓冲区，如果B_DIRTY被设置将缓冲区的内容写到磁盘，如果B_VALID没有被设置，从磁盘中读出数据到缓冲区。`iderw`维护了一个等待中的磁盘请求队列,然后用中断来指明哪一个请求已经完成。`iderw`将缓冲区`b`送到队列的末尾。如果这个缓冲区在队首, iderw通过`idestart`将它送到磁盘上;在其他情况下,一个缓冲区被开始处理当且仅当它前面的缓冲区被处理完毕。

  `idestart`依据标志位发出关于缓冲区所在设备和扇区的读或者写操作。如果操作是一个写操作, idestart必须提供数据而在写出到磁盘完成后会发出一个中断。如果操作是一个读操作,则发出一个代表数据就绪的中断,然后中断处理程序会读出数据。

  磁盘会完成自己的操作后会触发一个中断，trap会调用`ideintr`来处理它。`ideintr`查询队列中的第一个缓冲区,看正在发生什么操作。如果该缓冲区正在被读入并且磁盘控制器有数据在等待,`ideintr`就会调用insl将数据读入缓冲区。现在缓冲区已经就绪了: ideintr设置B_VALID,清除B_DIRTY,唤醒任何一个睡眠在这个缓冲区上的进程。最终, 	ideintr将下一个等待中的缓冲区传递给磁盘。

#### 4. 锁

- `spinlock.h`定义了锁的结构

  ```c
  /* 锁的数据结构 */
  struct spinlock {
    uint locked;       // 0:可以上锁，非0：已经上锁

    // For debugging:
    char *name;        // Name of lock.
    struct cpu *cpu;   // The cpu holding the lock.
    uint pcs[10];      // The call stack (an array of program counters)
                       // that locked the lock.
  };
  ```

- spinlock.c定义了锁的操作

  函数`acquire`用来获得锁。它使用原子指令`xchg`将`lk->locked`置1来上锁,为了便于调试还会记录获得锁的CPU和栈的信息。

  函数`release`用来释放锁。它清除调试信息，并将`lk->locked`置0.

#### 5. 调度

xv6的调度算法是轮转法(round robin)，仅仅是让每个进程轮流执行。每个CPU都有自己的调度线程，调度就发生在调度线程中。

- `swtch.S`是上下文切换的代码。存在两种类型的切换：内核线程和调度器线程之间的切换，用户进程和内核线程之间的切换。

  在`proc.h`里定义了上下文的结构`struct context`，它决定了上下文切换的次序。

  ```c
  struct context {
    uint edi;
    uint esi;
    uint ebx;
    uint ebp;
    uint eip;
  };
  ```

  首先用%eax保存`struct context **old`的指针，%edx保存`struct context *new`的值，然后依次将%ebp, %ebx, %esi, %edi入栈，然后将当前%esp保存在`*old`中，将%esp切换到`new`，最后将之前保存的寄存器的值出栈并返回。

  xv6调用`swtch`函数的地方只有两处：一处是是在`proc.c`的`sched`函数里，从内核线程切换到某个CPU的调度器线程；一处是在`proc.c`的`scheduler`函数里，从某个CPU的调度器线程切换到内核线程。

- `proc.c`里的调度部分，共行程序sched和scheduler。

  进程释放CPU的过程：获得进程表的锁`ptable.lock`，释放拥有的其它锁，修改自己的状态`proc->state`，调用`sched`。`yield`, `sleep`和`exit`都遵循了这个约定。

  `sched`首先检查进程此时是否持有锁，CPU是否关闭了中断。然后调用swtch切换到调度器上下文 ，返回到`mycpu()->scheduler`中。

  `scheduler`则是一个双层for循环。在内层循环中找到一个可运行的进程(p->state==RUNNABLE)，然后将当前cpu的`c->proc`设为该进程，用`switchuvm`切换到该进程的页表，标记该进程为`RUNNING`，再调用`swtch`切换到该进程中运行。在外层循环中要释放锁并显式地允许中断。因为如果一个闲置的调度器如果一直持有锁，其它CPU就不能进行调度操作了，如果不允许中断其它进程的I/O就永远无法到达了。

#### 6. 文件系统

xv6的文件系统分6层，如下表所示。

| 表现形式 | 实现形式     | 作用                                     |
| -------- | ------------ | ---------------------------------------- |
| 系统调用 | 文件描述符   | 将管道，设备，文件等抽象为文件系统的接口 |
| 路径名   | 递归查找     | 实现了文件的路径                         |
| 目录     | 目录结点     | 由目录项组成的结点                       |
| 文件     | 结点和数据块 | 由结点和数据块组成的文件                 |
| 事务     | 会话         | 通过会话的方式保证磁盘操作的原子性       |
| 块       | 缓存         | 对硬盘的缓存                             |

硬盘上文件系统的结构如下表所示，可与`fs.h`对照阅读。

| 块号   | 存放内容 | 作用                           |
| ------ | -------- | ------------------------------ |
| 最后   | log      | 日志块，是会话层的一部分       |
| 大部分 | data     | 数据块，保存了文件和目录的内容 |
|        | bit map  | 空闲块位图                     |
| 2      | inodes   | 存放结点                       |
| 1      | super    | 超级块，存放文件系统元信息     |
| 0      | boot     | 存放bootloader，文件系统不用   |

- `bio.c`是块缓冲层的代码，使用的是近期最少使用算法(LRU)。详见：第六章 **代码:块缓冲**
- `log.c`是日志层的代码。详见：第六章 **代码:日志**
- `fs.c`的balloc和bfree函数。详见：第六章 **代码:块分配器**
- `fs.c`中对i结点的操作。详见：第六章 **代码:i节点**
- `fs.c`中bmap函数。详见：第六章 **代码:i节点内容**
- `fs.c`中目录层的代码。详见：第六章 **代码:目录层**
- `fs.c`中路径名的操作。详见：第六章 **代码:路径名**
- `file.c`是文件描述符层的代码。详见：第六章 **文件描述符层**
- `sysfile.c`是对文件系统的系统调用。详见：第六章 **代码:系统调用**


- `pipe.c`是管道的代码。详见：第五章 **代码:管道**

