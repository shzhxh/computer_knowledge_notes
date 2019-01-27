**Porting Hyperkernel to the ARM Architecture**

Dylan Johnson(University of Washington)

#### 简介

在开发出Hyperkernel在X86下的原型后，ARM的优越设计促使他们把Hyperkernel移植到ARM v8-A 架构中。

#### 相关工作

对移植工作的讨论。

##### OS设计

Hyperkernel有些地方与Dune的设计比较相似，比如每个进程都是虚拟化的，这样使得可以直接控制诸如中断向量之类的硬件特性。Dune使用一个小的Linux内核模块来协调几个运行的虚拟进程，用一个用户级的库来辅助管理新暴露的资源。

Hyperkernel也借鉴了外核的设计。外核是一种操作系统内核，它提供了操作系统抽象的用户级管理。外核允许用户程序管理硬件资源，它则为整个系统提供资源策略。像Aegis和Xok这样的传统外核实现提供了用户代码在内核级运行的底层抽象，由于共享了内存，所以难以保证故障隔离。Hyperkernel则是使用虚拟机来提供底层抽象的，硬件虚拟化带来的性能提升使这种设计成为可能。

##### Hypervisor设计

KVM-ARM 是Linux主线里的一个hypervisor。KVM-ARM使用ARM编程模型以一种非常规的方式减少和简化了代码需求。具体来说它使用了一个叫split-mode虚拟化的设计，只有一小部分hypervisor执行使用了ARM虚拟化扩展，大部分都是返回到Linux里充分利用已构建好的内核机制。

#### ARM架构概览

本文所说的ARM架构特指ARM v8-A架构。它为应用程序，操作系统和hypervisors定义了丰富的编程模型。

##### 异常的级别

| CPU模式 | 运行的程序     | 特点                                         |
| ------- | -------------- | -------------------------------------------- |
| EL0     | 应用程序       | 有限暴露系统配置寄存器和异常处理，以隔离进程 |
| EL1     | 操作系统       | 暴露异常处理、页表修改和大量的系统配置选项   |
| EL2     | Hypervisor     | 为虚拟化提供硬件支持，可以模拟或转发敏感操作 |
| EL3     | Secure Monitor | 最高特权级，可以在非安全和安全执行状态间切换 |

每个CPU模式都被称为一个异常级别(exception level)，它决定了当前执行程序的特权级。由叫做异常的同步或异步事件来引发异常级别的转换。一个异常只能进入到更高级别或同级的异常中，所以异常返回时异常级别只能是降低或保持不变。每个异常级别都支持两种执行状态，AArch32和AArch64。AArch32使用32位且兼容ARMv7-A架构，AArch64使用64位且仅为ARMv8的新特性。Hypervisor目前只支持Arch64，以简化它的实现。

##### 虚拟内存模型

ARM的每个异常级别都有页表基址寄存器(called translation table base registers, TTBRs)。而X86所有CPU模式都共用一个页表基址寄存器。EL0和EL1使用TTBR0_EL1(在低地址)和TTBR1_EL1(在高地址)共用一个地址空间，两个寄存器都在EL1的控制下。这样的设计为划分内核和用户地址空间提供了一个方便的机制。EL2和EL3有各自的地址空间，其对应的页表基址寄存器分别为TTBR0_EL2、TTBR0_EL3。

内存虚拟化由地址转换的第二阶段提供，类似于X86里的扩展页表(EPT)。在EL1中运行的虚拟机首先会用它自己的页表进行第一次地址转化，从虚拟地址到中间物理地址(IPA)。接下来，第二阶段将会使用EL2里的hypervisor提供的页表，将IPAs转换为物理地址。hypervisor可以使用第二阶段的转换来保护MMIO外围设备不受VM的直接控制，因为路由到EL2的数据中止异常会强制执行。

##### TLB

ARM的虚拟内存模型减少了不必须的TLB刷新。比如，ARM TLB可以依据当前的异常级别自动匹配入口，避免了产生异常的时候刷新TLB。这和X86是类似的，vm退出到root模式的时候不会引发TLB刷新。此外，ARM还定义了一个唯一的应用程序特定标识符(ASID)，由OS分配给每个用户进程。ASID通过TTBR0_EL1的前16位和硬件通信。在地址转换的过程中，满足TLB入口的硬件使用ASID，避免了上下文切换的时候刷新TLB。X86也有类似的机制，叫做进程上下文标识符(PCID)。

##### 中断

ARM上的中断是由通用中断控制器(GIC)来管理和路由的。GIC架构定义了中断的生成、路由和优先级的机制。GICv3是第3代GIC架构，它定义了两个组件：distributor和redistributor。distributor只有一个，它处理的是全局中断和处理器间中断。而每个CPU都有一个redistributor，它处理的是本地中断。与distributor的交互是通过MMIO进行的。与redistributor的交互是通过一组配置寄存器来进行的。

GICv3为每个CPU定义了一个虚拟GIC(vGIC)接口，它向hypervisor提供虚拟中断管理功能。虚拟中断把所有的物理中断路由到EL2。在目标VM分配到物理CPU之前，the hypervisor signals for a virtual interrupt to be delivered once execution has returned to EL1。如果VM尝试通过MMIO来配置GIC distributor，第二阶段的转换会把执行陷入到EL2，使得hypervisor能模拟那个操作。VM对GIC寄存器的访问则被路由到一组虚拟寄存器，它们控制了虚拟中断的分发。在虚拟CPU上下文切换时，hypervisor必须保存可写的虚拟寄存器状态，挂起的虚拟中断的状态，和VM特定的虚拟中断配置。

##### 计数器

每个ARM CPU都有两个计数器，物理计数器和虚拟计数器。虚拟计数器的值就是物理计数器减去一个偏移量。 These two counters power a set of generic timers accessible from each CPU mode.物理计数器驱动了一个EL1计时器和一个EL2计时器，虚拟计数器则驱动了一个虚拟计时器。所有的3种计时器都支持upcounter和downcounter中断，它们都只会为其指定的CPU生成一个中断。传统上，hypervisor会使用EL1计时器抢占虚拟机执行，EL1中的虚拟机则使用虚拟计时器来抢占用户进程。 To avoid virtual machines from observing the physical passage of time,从EL1到EL1计时器的访问都被陷入到EL2。

#### ARM Hyperkernel 设计

##### 异常的级别

和X86类似，ARM版的Hyperkernel也需要把内核和用户地址空间分离。此外，它还可以向用户空间容易地输出低级别的硬件资源，比如页表和异常管理。

EL0, EL1和EL2都可以放置用户进程。EL0不能访问大部分的配置寄存器，使得它不能很好地控制低级别的硬件。把用户进程放在EL2也是很难的。一是ASIDs无法分配给EL2的进程，这将导致每次上下文切换都会刷新TLB。二是EL2的用户进程对TTBR0_EL2有完全的控制权，恶意进程可以手动更改页表来实现对物理内存的任意访问。三是使vGIC的功能变的复杂，因为虚拟中断只能路由到EL1,EL2的中断处理只能通过系统调用的方式实现。所以，用户空间放到EL1是最佳选择。

由于用户进程放在EL1,Hyperkernel只能放在EL2或EL3。EL3里诸如安全模式这样的特性会使Hyperkernel的实现不必要的复杂化，所以Hyperkernel放在EL2里。

##### 虚拟内存模型

最初，Hyperkernel被设计为在EL2执行时不使用MMU。这样可以消除对内核页表的需求，还使得内核和用户空间的隔离变的更容易验证。不幸的是，在EL2禁用ARM的MMU也就永久的禁用了数据缓存，这对系统性能影响极大。所以，Hyperkernel会在EL2自己的地址空间执行。

用户空间只使用TTBR0_EL1，超过TTBR0_EL1的虚拟内存访问都将进入Hyperkernel。用户进程可以通过系统调用来修改它们自己的页表，直接修改TTBR0_EL1则会陷入到Hyperkernel以避免打破内核到用户空间之间的隔离。

用户进程无法访问它们自己的地址空间，而虚拟机对所有的地址都有完全的控制。一般hypervisor通过第二阶段的转换实现此抽象。Hyperkernel则没有使用第二阶段的地址转换，因为它通过页映射系统调用控制控制着EL1的虚拟内存。一些Hypervisor虚拟化的实现依赖于第二阶段的转换，Hyperkernel则不在MMIO上映射任何虚拟页，这样就可以限制那些内存区域的访问了。

##### TLB

Hyperkernel使用ASIDs的优势为每一个进程管理一个ASID。当进程被分配到CPU上时，TTBR0_EL1被更新为正确的ASID.

##### 中断

Hyperkernel配置系统把所有的中断都路由到了EL2。如果最终目标是EL1里的进程，则Hyperkernel把它加到那个进程的中断列表里，否则Hyperkernel就直接处理那个中断。当一个进程被调度前它的中断被转发给vGIC，当进程返回时vGIC会生成虚拟中断。在Hyperkernel的用户进程里处理虚拟中断和在通用内核里处理中断没有不同之处。

##### 计数器

A user processes view of physical time can advance significantly between the execution of two instructions. In contrast, a virtual machine requires timers that will only advance when one of its vCPUs is scheduled. Hyperkernel删除了虚拟计时器逻辑，节省了转换到EL2的成本。用户进程对EL1计时器有完全的控制权。EL2计时器被Hyperkernel调度器用来抢占进程。

#### 实现

描述了Hyperkernel在ARM上的实现，以及X86版本与ARM版本之间的差异。

##### 页表管理

对于ARM版本的Hyperkernel, x86页表管理代码被大量重用。这是因为两个版本的Hyperkernel都使用4KB的页面，48位虚拟地址，以及4级虚拟地址转换。漫游页表、插入新的页表条目或分配新页面的函数只需要进行小的修改就可以在ARM上工作。对这些函数最大的更改是在页表条目中提取权限的格式和设置。幸运的是，两个描述符格式的权限都是相似的，从一种格式转换到另一种格式只需重新安排权限位。

##### 进程管理

X86和ARM上进程保存的信息都是一样的，它们是进程状态、内核栈、CPU状态、进程号、文件描述符、挂起的中断、IPC信息。但是在CPU状态里保存的内容是不一样的，X86保存的是虚拟机控制结构(VMCS)，ARM中保存的只是一个简单的结构体。当从EL1进入EL2时(上下文切换、hypercall或trap)，CPU所需状态保存在结构体中。当返回EL1时，这个状态被复制回CPU。

##### 驱动与组件

许多超内核功能需要新的驱动程序和组件来与ARM硬件通信。例如，SMP多核管理操作(如启用CPU核心)是通过称为power state coordination interface (PSCI)的电源管理系统实现的。使用EL3实现的系统通过同步异常将PSCI管道暴露给EL3。Hyperkernel为EL3生成PSCI调用，EL3将执行必要的MMIO通信以启用或禁用CPU内核。实现的其他组件包括用于串行通信的PL011 UART驱动程序、一个用于调度的简单计时器模块，以及一个用于中断管理的GICv3驱动程序。

##### QEMU to HiKey

QEMU精确模拟了真实的ARM硬件。当在真实的HiKey板上运行的时候，对Hyperkernel的唯一修改DRAM的基址。

##### Bootloader

在真实硬件上运行Hyperkernel需要一个bootloader。他们使用Linaro EFI工具链创建了一个自己定制的EFI bootloader。这个bootloader只是把内核代码放在DRAM的起始位置，然后在内核后面放的设备树的二进制对象，并跳转到Hyperkernel开始执行。

#### 实验与结果

实验展示和ARM hypercalls的性能展示。

ARM版本的Hyperkernel包括内核和init进程的简单实现。还需要一个完整的用户空间实现来来运行benchmark。然而当前的benchmark还不完善。典型的ARM操作系统是通过**svc**指令生成系统调用与用户进程交互的。然而由于Hyperkernel运行在EL2层，所以用户进程只能使用**hvc**指令与内核交互。测试结果表明，以上改变不会造成性能下降。

#### 总结

Hyperkernel的ARM版本有助于实现系统优化。

