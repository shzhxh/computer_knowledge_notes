### 进程管理的数据结构

```c
struct proc_struct {
    enum proc_state state;                      // 进程在其生命周期里的所有状态，有4种：uninit,sleeping,runnable,zombie
    int pid;                                    // 进程号
    int runs;                                   // 进程的运行时
    uintptr_t kstack;                           // 进程的内核栈
    volatile bool need_resched;                 // 非0则调用调度函数
    struct proc_struct *parent;                 // 父进程
    struct mm_struct *mm;                       // 进程的内存控制域
    struct context context;                     // 进程上下文，包括了返回地址，栈指针，保存寄存器的值
    struct trapframe *tf;                       // 当前中断的trapframe
    uintptr_t cr3;                              // 相当于RISC-V的satp寄存器
    uint32_t flags;                             // 如果是PF_EXITING,则表示杀掉此进程
    char name[PROC_NAME_LEN + 1];               // 进程名
    list_entry_t list_link;                     // 进程的链表 
    list_entry_t hash_link;                     // 进程的哈希表
    int exit_code;                              // 将会发给父进程的退出码
    uint32_t wait_state;                        // 等待状态：等待子进程或等待中断
    struct proc_struct *cptr, *yptr, *optr;     // cptr:子进程，yptr:弟进程，optr:兄进程
};
```



### 进程管理的函数
#### do_execve

重新设置内存空间，然后将程序的二进制代码载入内存

#### load_icode

将文件的代码部分载入内存  

1. 创建一个新的mm_struct结构体mm用以描述进程在内存中的分配情况。  
2. 然后为mm分配一个新的PDT。  
3. 将TEXT段，DATA段，BSS段载入到内存中。  
  1. elf文件刚开始的一部分是文件头，描述了文件的整体结构，需要先读取elf文件头中的信息。  
  2. 通过elf文件头查到程序头的位置和数量，程序头描述了TEXT段和DATA段的信息，所以需要再读取程序头中的信息。程序头的标志项(p_flags)共有3种选项：ELF_PF_X可执行，ELF_PF_W可写，ELF_PF_R可读。对应了虚拟内存区域(vma)的三个选项：VM_EXEC,VM_WRITE,VM_READ。同时也决定了页表入口(PTE)的标志：PTE_U用户可访问，PTE_W可写。  
  3. 依据每个程序头描述的起始地址、大小和标志，创建vma_struct结构体。
  4. 依据每个程序头描述的起始地址、大小和标志分配内存页，将文件相应内容载入到内存中。  
  5. ​
4. 依据USTACKTOP和USTACKSIZE，创建用户栈的vma_struct结构体。然后为用户栈申请内存页。  
5. 依据mm设置当前进程的mm和cr3，设置cr3寄存器的值。
6. 依据argc，将argv输入到用户栈中。
7. 为当前进程的trapframe结构体的指针tf赋值。
8. 如果上面的某步出现错误，应该将之前的设置全部清空。

#### do_exit

释放进程的几乎所有内存，然后将进程状态设置为PROC_ZOMBIE

#### do_fork

为子进程分配除CPU外的其它资源。子进程通过schedule调度函数获取CPU资源。最终是通过switch_to汇编代码码实现的。

#####      alloc_proc

​    为进程控制块(proc_struct)分配内存并初始化

#####     setup_kstack 

​     为内核线程分配栈空间

#####     copy_mm

​    共享或复制父进程的`mm_struct`

#####         copy_thread

​     初始化子进程的trapframe     

#####        get_pid

​       分配进程号

#####        hash_proc

​       将子进程加入hash_list

#####        set_links

​        设置子进程和其它进程之间的关系

#### do_kill

杀死进程  
原理：将进程的flags设置为PF_EXITING

#### do_sleep

将进程状态设置为PROC_SLEEPING

#### do_wait

`int do_wait(int pid, int *code_store)`

如果pid是当前进程的子进程，且其状态为PROC_ZOMBIE，则释放此子进程占用的内存空间。否则，会遍历当前进程的子进程，直到找到一个状态为Zombie的并将其释放。

#####       put_kstack

​      释放指定进程的内核栈所占用的内存空间。

#### do_yield

将当前进程设置为可调度

#### kernel_thread

`int kernel_thread(int (*fn)(void *), void *arg, uint32_t clone_flags)`

为函数`fn`创建内核线程。其实就是do_fork函数的封装，为函数`fn`分配除CPU外的资源。

#### kernel_execve

`static int kernel_execve(const char *name, unsigned char *binary, size_t size)`

使用系统调用SYS_exec来执行一个用户态的程序，本函数只被内核线程user_main调用。

#### find_proc

`struct proc_struct *find_proc(int pid)`

寻找进程号pid对应的进程。返回其PCB的指针。

#### proc_run

`void proc_run(struct proc_struct *proc)`

使进程proc在CPU上运行起来。本函数只被schedule函数调用。

### 进程与文件
进程是将文件的内容载入到内存中，进程看到的文件系统是虚拟文件层(VFS)。在虚拟文件层的抽象中，看到的基本数据结构是文件(file)，文件之间组织为树状的形式，文件本身也表现为某种抽象结构。要想将文件正确的载入内存，需要知道文件路径、文件结构和内存位置。

### Copy On Write

