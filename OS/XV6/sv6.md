#### 来源

[twd2](https://github.com/twd2/sv6)

#### 编译过程

1. 将目录`kernel`下的c文件编译为目录`o.qemu/kernel`下的obj文件。
2. 使用python脚本`tools/syscalls.py`，从`kernel`目录下的文件里提取信息，生成`o.qemu/include`和`o.qemu/lib`下的文件。
3. `tools/mkfs.c`编译为`o.qemu/tools/mkfs`。
4. `bin`文件夹下的文件编译为`o.qemu/bin`文件夹下的文件。
5. `lib`文件夹下的文件编译为`o.qemu/lib`文件夹下的文件。
6. 将`o.qemu/kernel`目录下的所有文件链接为`kernel.elf`。
7. 以kernel.elf为payload，编译生成bbl，另存为`o.qemu/bbl.elf`
8. `sfs.img`的生成过程应该是和xv6下是一样的。

#### 运行过程

start->cmain->mpboot

##### start:内核入口

KBASE是内核虚拟地址在物理内存中的起始地址。链接器是从KBASE+2M的位置执行的，而加载器把内核加载到了2M的位置，所以必须要把低地址映射到高地址。KBASE的值要与include/memlayout.h和kernel/kernel.ld的定义相符。

##### 其它

设备树信息是在riscv-pk/bbl/bbl.c里使用fdt_print函数实现的。

memory map信息是在kernel/main.cc里initphysmem函数实现的。

page_info map信息是在initpageinfo里实现的。

stvec信息是在initrap里实现的。

cmdline信息是在initcmdline里实现的。

kalloc信息是在initkalloc里实现的。

initsamp和initnode信息分别是在各自函数里实现的。

cpu1~cpu7的信息可能要从内核线程里查。

cpu0的信息来自于cmain函数本身。

#### SMP

##### initnuma

##### initpercpu

##### initcpus

##### bootothers

##### sbi函数

- 在include/sbi.h里定义，最终通过ecall指令在bbl里实现具体的功能。
- 在kernel/console.cc里用sbi_console_putchar实现函数consputc的功能。
- 在kernel/ipi.cc里用**sbi_send_ipi**实现类**ipi_call**的start方法。
- 在kernel/sampler.cc里用sbi_console_putchar实现wdcheck函数实现输出某些字符的功能。
- 在kernel/kbd.cc里用sbi_console_getchar实现kbdgetc的功能。
- 在kernel/main.cc里用sbi_shutdown实现halt函数的功能。
- 在kernel/trap.cc里trap函数使用了sbi_clear_ipi。
- 在kernel/timer.cc里sbi_set_timer实现了函数timer_set_next_event的功能。
- 在kernel/hwvm.cc里类**batched_shootdown**的perform方法使用了sbi_remote_sfence_vma。

##### 类ipi_call

- 在include/ipi.hh里定义了类ipi_call。
- 在kernel/ipi.cc里定义了类ipi_call的start方法和run_on方法。定义了ipi_call的两个对象head和tail。函数on_ipicall使用了类ipi_call，管理当前CPU的中断调用。

##### 类batched_shootdown

- 类batched_shootdown是一个TLB命中收集器，它不追踪任何东西，但作为结果可以与其它TLB命中作为一批处理。


- 在include/hwvm.hh里定义了类batched_shootdown。
- 在kernel/hwvm.cc里定义了类batched_shootdown的perform和on_ipi方法。perform方法的作用是刷新所有核的TLB。on_ipi处理一个刷新TLB的IPI的接收。

#### 调度

##### idleloop

##### initsched

作用：推测是初始化调度器运行队列。

数据结构devsw看起来是设备操作的框架。

```c
struct devsw {
  int (*read)(mdev*, char*, u32);
  int (*pread)(mdev*, char*, u32, u32);
  int (*write)(mdev*, const char*, u32);
  int (*pwrite)(mdev*, const char*, u32, u32);
  void (*stat)(mdev*, struct stat*);
};
```

major.h里的宏定义，推测是对主要设备的编号。

```c
#define MAJ_CONSOLE  1		// szx:控制台
#define MAJ_NETIF    2		// szx：网络接口
#define MAJ_SAMPLER  3		// szx：釆样器
#define MAJ_LOCKSTAT 4		// szx:锁状态
#define MAJ_STAT     5		// szx:主状态
#define MAJ_CMDLINE  6		// szx:命令行
#define MAJ_GC       7		// szx:猜不到什么意思
#define MAJ_KCONFIG  8		// szx:内核配置
#define MAJ_KSTATS   9		// szx:内核状态
#define MAJ_KMEMSTATS 10	// szx:内核内存状态
#define MAJ_MFSSTATS 11		// szx:文件状态
```

类window_stream继承自print_stream，从一个窗口输出中捕获流，用于简单的设备读取。

thesched_dir是类sched_dir的一个对象。其scheddump方法是打印各个核的信息。

statread函数，看起来是在打印处理器的信息。

#### 在FU540-C000中运行sv6

```
git clone https://github.com/twd2/sv6.git
make qemu	# 确保能在qemu中运行
make sdimg	# 生成镜像，是o.qemu/sd.img
dd if=o.qemu/sd.img of=/dev/XXX	# 写入到SD卡
# 最后，在FU540-C000上启动
```

##### 问题解决

- `dd: error writing '/dev/sdc1': No space left on device`

  这是因为源镜像比sdc1大引起的，解决方法就是给sdc1分配更大的空间

- 镜像写入SD卡后，无法在FU540-C000上启动

  首先，Linux的镜像写入SD卡可以启动，而sv6的镜像写入SD卡不能启动，这表明问题不是出在其它方面，而只是镜像的问题。

  其次，sv6的镜像可以在qemu上运行，这表明问题应该出在qemu镜像向sd卡镜像转换的过程中。

  最后，发现Linux是使用bbl.bin作为镜像的，而sv6是把bbl.bin转换成sd.img，以sd.img为镜像的。当我尝试用sv6的bbl.bin作为镜像时，可以正常在开发板上启动sv6了。


