### 碰到的问题

1. 使用虚拟化技术进行形式化验证的好处是什么？
2. 论文中提到的identity mapping是什么意思？
3. 在`kernel/pmap.c`里的`e820_multiboot`函数，其数据结构`multiboot`并未进行初始化怎么就开始使用了？其初始化的代码在`kernel/locore.S`里`start_multiboot`例程。

### 编译过程

- 所有的编译目标都在o.x86_64里。
- hv6的文件编译到o.x86_64/hv6下，是hv6内核的实现。
- hv6/user的文件编译到o.x86_64/hv6/user文件夹下，是用户态程序的实现。
- lib的文件编译到o.x86_64/lib下，看起来是一些与I/O相关的库。
- drivers的文件编译到o.x86_64/drivers下，看起来是一些硬件驱动。
- include/uapi/machine/trap_genassym.c编译为目标文件，再用scripts/genassym.sh生成o.x86_64/include/uapi/machine/trap_assym.h文件。
- 生成用户空间的库o.x86_64/hv6/ulib，以hv6/user/lib/ulib.lds为链接脚本，包含hv6/user/lib下的目标文件。
- 利用库ulib，生成用户空间的可执行文件，从hv6/user到o.x86_64/hv6/user。
- 生成库o.x86_64/hv6/liblwip.a，文件来自于hv6/user/lwip下的目标文件。
- 生成fs.img，使用o.x86_64/hv6/user/fs/mkfs将之前编译出的用户空间的可执行文件放到文件系统fs.img中。
- 生成init，其链接文件来自于x86_64/hv6/user、lib和drivers。
- 将kernel下的文件编译到o.x86_64/kernel下。
- 生成o.x86_64/kernel/kernel.lds，依据kernel/kernel.lds.S。
- 生成内核hv6.elf，依据上步生成的链接脚本kernel.lds，其链接文件来自于o.x86_64/hv6、lib、kernel和drivers。
- hv6.bin，hv6.asm是从hv6.elf来的，ulib.asm是从ulib来的。

### 运行过程

- 依据从o.x86_64/hv6.asm读到的内容来看，内核的起始代码在标志`_start`处，但尚未找到代码位置。

- 接下来执行kernel/locore.S里的`start_multiboot`初始化结构体multiboot_info并设置BSP，然后执行`start_common`开启保护模式，并最终执行c代码里的main函数。

- 接下来执行hv6/main.c里的`main`函数

  `arch_init`处理与硬件相关的代码。

  `porte9_init`初始化e9端口(uart设备)，即e9端口放到设备列表里(是一个链表)

  `cga_init`初始化cga设备，即把cga的console_device放到设备列表里

  `tsc_init`初始化tsc寄存器(time stamp counter)，即先初始化计数器0，再计算出时钟频率`tsc_mhz`。

  `trap_init`初始化中断向量表，并将其保存到idt寄存器中。

  `cpuid_init`获取处理器信息，将其保存在`features[]`数组中。

  `acpi_init`将acpi_table_header的地址写入到acpi_tables。首先是找到rsdp的地址，再通过rsdp找到rsdt，再通过rsdt找到hdr。

  `pmap_init`首先用函数`e820_multiboot`把可用内存加到结构体`bootmem_map`里，把保留内存从结构体`bootmem_map`里去除。然后把内核占用的内存和内核以下的内存都从`bootmem_map`里去除。最后再把cr3寄存器的值写入到cr3寄存器(**看起来是多此一举，不明白为什么这么操作？**)。

  `mtrr_init`读取mtrr寄存器组，列出无效的内存区域。

  `xapic_init`设置apic。

  `fpu_init`设置cr0和cr4寄存器的fpu部分。

  `hvm_init`如果是AMD的虚拟化技术则执行`svm_init`，如果是INTEL的虚拟化技术则执行`vmx_init`。

  `iommu_init`

  `iommu_early_set_dev_region`

  允许用户空间访问某些端口(com,kbd,crt)

  保留向量0-31

  设置pci回调

  设置抢占函数

  设置外中断函数

  设置triple fault函数

  写msr寄存器

- `vm_init`是把结构体`page_desc`组成的数组`page_desc_table`里的每一项都放到双向链表里。

- `user_init`看起来是创建用户进程

- `print_config`打印一些配置常量

- `print_version`打印commit信息

- `check_invariants检查进程和物理页的不变量

- `run_current`看来是运行当前进程

- hv6/user/initcode.S也许是用户进程的起始代码，从_start开始

- hv6/user/init.c的`main`函数，开启文件服务、网络服务和shell

### 逻辑结构

#### 系统调用(55)

在hv6/syscall.c里的`syscalls`数组定义了系统调用号与系统调用之间的映射关系

##### vm(16, 31%)

- vm.c(16)：sys_map系列是分配，sys_alloc系列是分配，sys_free系列是释放

##### proc(11, 20%)

- proc.c(5)：`sys_kill`，`sys_reap`，`sys_reparent`，`sys_set_proc_name`，`sys_set_runnable`
- entry.S(6)：`sys_clone`，`sys_switch`，`sys_send`，`sys_recv`，`sys_call`，`sys_reply_wait`

##### fd(5, 9%)

- fd.c(5)

  `sys_create`创建文件

  `sys_close`关文件

  `sys_dup`和`sys_dup2`都是复制，sys_dup2只比sys_dup多做了两个条件判断，两者都可以看作是对函数`set_fd`的封装，`set_fd`是将当前进程的打开的文件oldfd复制到进程pid的打开文件newfd。

  `sys_lseek`文件内查找

##### device(17, 31%)

- device.c(14)：sys_alloc系统是分配设备，sys_reclaim系列是回收设备，sys_map系列是映射设备，`sys_ack_intr`是清除当前进程PCB里initr的某位
- ioport.c(3)：`sys_alloc_io_bitmap`端口映射到其所有者的pid，`sys_alloc_port`看起来是分配端口，`sys_reclaim_port`看起来是回收端口

##### debug(6, 11%)

- main.c(1)：`sys_debug_exit`测试qemu的`-device isa-debug-exit`
- syscall.c(4)：`sys_nop`错误的系统调用号，`sys_debug_print_console`写终端，`sys_debug_print_screen`写cga，`sys_debug_dmesg`看起来是读系统日志
- sysctl.c(1)：`sys_debug_sysctl`看起来是返回一些系统参数

#### 进程管理

- 每个进程的信息都放在结构体`proc`里，所有`proc`都放在一个数组`proc_table`里，最多可以有64个进程，变量pid代表进程在数组内的编号。`ofile`是进程内打开的文件组成的数组，变量fd是文件在数组内的编号。`nr_fds`代表进程内打开的文件数。
- 全局变量`current`代表当前进程。
- 只有`get_proc`操作了数组`proc_table`，通过pid获取PCB的地址。

#### 文件管理

- 每个文件的信息都放在结构体`file`里，所有`file`都放在一个数组`file_table`里，最多可以有128个文件，变量fn代表文件在数组file_table里的编号。`refcnt`代表文件的引用数，当引用数为0时需要对其它数据结构执行初始化操作。

- fd是进程内打开文件的编号，它有三个操作：`clear_fd`关闭进程内打开的文件，`get_fd`获取打开文件所对应的fn，`set_fd`为fn和fd创建映射关系。

#### 虚拟化

- vmx.c：`vmx_init`函数执行`vmxon`进入vmx操作模式。
- vmx.h：Intel的虚拟化技术。结构体`vmcs`前4个字节是VMCS版本标识，接下来4字节表示中止发生的原因，接下来都是数据区。数据区由3个部分组成：控制区域(虚拟执行控制域，VMExit控制域，VMEntry控制域，VMExit信息域)，Guest区(寄存器状态，非寄存器状态)，Host区(主要是VMM运行时的状态)。
- svm.c：AMD的虚拟化技术
- hvm.c：屏蔽了Intel和AMD虚拟化技术的差异，是对虚拟化技术的顶层抽象。