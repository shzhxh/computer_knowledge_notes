#### 从内核态到用户态

在xv6启动的过程中，0号核在main函数里会执行userinit函数，标记initcode处的数据会复制到第一个用户进程的内存空间。标记initcode的数字是一段代码，它对应的内容在user/initcode.S，是要把文件init的内容放到内存里以用户态去执行。文件init编译自user/init.c。

user/init.c把文件sh放到内存里执行。文件sh编译自user/sh.c，把用户输入的命令调入内存中执行，它就是xv6的shell。shell就是不断地使用getcmd函数读取命令行的输入，然后使用runcmd函数来执行命令行的输入。

#### 从用户态到内核态

系统调用怎么从用户态到的内核态呢？文件user/usys.pl生成user/usys.S，usys.S就是用来完成从用户态到内核态的代码。从代码可见，它是通过ecall产生一个异常来进入内核态。只要把usys.S生成的二进制文件链接进用户程序里，就可以使用系统调用了。

#### 进程和内存

相关系统调用的实现请看kernel/sysproc.c。

##### fork

文件位置：kernel/proc.c。作用：创建一个进程。

fork函数一次调用两次返回，这很怪异，因为一般的调用都是只返回一次的。fork从父进程返回很正常，但为什么会从子进程返回呢？这是因为子进程继承了父进程的所有资源，这一句`*(np->tf) = *(p->tf)`相当于子进程复制了父进程运行的“快照”，子进程里也相当于进行了一个fork函数且从下一句开始执行。

1. p为当前进程，np为子进程。
2. uvmcopy把父进程的内存复制给子进程。
3. `np->tf->a0 = 0`保证了子进程返回0，因为寄存器a0保存了函数的返回值。
4. `return pid`对于子进程来说，在汇编层面应该实际执行的返回寄存器a0的值。

##### exit

文件位置：kernel/proc.c。作用：让调用它的进程停止运行。

##### wait

文件位置：kernel/proc.c。作用：等待当前进程的某个子进程退出。

##### exec

文件位置：kernel/exec.c。作用：把硬盘上的程序加载到内存里并执行之。

##### sbrk

文件位置：kernel/sysproc.c。作用：为进程分配内存。

#### 文件系统

相关系统调用的实现请看kernel/sysfile.c。

##### read

文件位置kernel/file.c。作用：从文件描述符所指向的文件读取n个字节。

##### write

文件位置kernel/file.c。作用：从文件描述符所指向的文件写入n个字节。

##### close

##### open

##### pipe

##### dup

##### chdir

##### mkdir

##### mknod

##### fstat

##### link

##### unlink

##### I/O重定向



