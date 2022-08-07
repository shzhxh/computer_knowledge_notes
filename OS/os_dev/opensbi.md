#### 简介

[OpenSBI](https://github.com/riscv/opensbi.git)是运行在M态的固件，用以支持H态或S态的程序，如hypervisor、bootloader或通用操作系统。

##### 什么是SBI

- SBI是RISC-V的管理员二进制接口

  像系统调用那样的调用规范

- SBI的作用

  减少不同操作系统之间重复的平台代码

  为OS提供通用驱动，屏蔽底层细节

  提供直接访问硬件资源的接口

- 由Unix平台规范工作组起草的规范

  [riscv-sbi-doc](https://github.com/riscv/riscv-sbi-doc)

##### 什么是OpenSBI

- OpenSBI是RISC-V SBI规范的开源实现
- 提供M模式下的**运行时**服务
- 提供对参考平台的支持
  - 通用的简单驱动(PLIC, CLINT, UART 8250)
  - 其它平台可以复用通用代码，并添加需要的驱动

##### 启动流程

1. CPU先从ROM中执行预设的代码(ZSBL:第零阶段的bootloader?)，此时处于机器模式，其作用是把LOADER加载到SRAM，进行SOC power-up和时钟设置
2. LOADER的代码被称为FSBL(第一阶段的bootload?)，FSBL可能是U-Boot_SPL、Coreboot、QEMU，此时处于机器模式，其作用是初始化内存(DDR)，把运行时和BOOTLOADRE加载到内存
   - 这里的运行时处于M模式，即为OpenSBI
3. BOOTLOADER运行于S模式，它提供对OS的一些支持，并最终启动OS(如U-Boot)
4. OS运行于S模式(如Linux)

##### 重要特性

- 分层结构以适应各种用例

  - 通用SBI库，抽象了底层细节(通常与外部固件和bootloader一起使用)
  - 平台相关的库(类似于核心库但多了特定驱动)
  - 平台相关的参考固件(三种类型的**运行时**固件)

- 支持广泛的硬件特性

  - RV32和RV64
  - Misaligned load/store管理
  - 缺失CSR模拟
  - 使用PMP来保护固件

- 可以使用Doxygen生成良好的文档

  ```shell
  sudo apt install doxygen doxygen-latex doxygen-doc doxygen-gui graphviz
  make docs	# 生成的refman.pdf在build/doc/latex目录
  ```


#### OpenSBI平台支持

##### 为什么要进行平台级支持

- 任何SBI实现都需要硬件相关的方法

- OpenSBI平台级的实现都是一组勾子，其形式为`struct sbi_platform`的数据结构实例

- 平台无关的通用OpenSBI代码都连接到静态库`libsbi.a`里

- 对于每个要支持的平台，我们都创建一个静态库`libplatsbi.a`

  `libplatsbi.a` = `libsbi.a` + `struct sbi_platform`

##### 支持的平台

- 详见`platform`目录
- 相关文档参考`docs/platform`目录

##### 添加平台支持

通过实现结构体`sbi_platform`来定义平台相关的钩子，这样libsbi.a就可以进行平台相关的操作了。

OpenSBI也提供了可启动的运行时固件，那些固件链接到了libplatsbi.a，保存在install/platform/<platform_subdir>/bin目录下。那些固件用以替代BBL。可以在include/sbi/sbi_platform.h读到关于`sbi_platform`和相关APIs的完整信息。

- 添加对平台xyz的支持

  ```
  1. 在platform目录下创建目录xyz
  2. 在xyz目录下创建配置文件config.mk
    - config.mk将提供编译flags，驱动，固件选择
    - 可参考platform/template/config.mk来创建config.mk
  3. 在xyz目录下创建对象文件objects.mk，用来列出要编译的对象
    - 可参考platform/template/objects.mk来创建objects.mk
  4. 在xyz目录创建源文件platform.c，以提供struct sbi_platform实例
    - 可参考platform/template/platform.c来创建platform.c
  ```

- xyz目录也可以放置在OpenSBI源码之外

##### 平台支持的编译选项

- `CROSS_COMPILE`需要在交叉编译的时候设置
- `make`仅编译通用OpenSBI(libsbi.a)
- `make PLATFORM=xyz`编译在OpenSBI源码里的平台相关OpenSBI(libplatsbi.a)
- `make PLATFORM_DIR=xyz_path`编译在OpenSBI源码外的平台相关OpenSBI(libpaltsbi.a)

#### OpenSBI用法

##### 引用固件

- OpenSBI提供了不同类型的参考固件，所有的平台相关
  - FW_PAYLOAD：下个启动阶段作为payload的固件
  - FW_JUMP：下个启动阶段使用静态跳转地址的固件
  - FW_DYNAMIC：下个启动阶段使用动态信息的固件
  
  > 以generic平台为例，make PLATFORM=generic会使顶层Makefile把platform/generic/config.mk包含进去，在这个config.mk里指定了FW_PAYLOAD、FW_JUMP、FW_DYNAMIC均为y，最后会把fw_dynamic.bin、fw_jump.bin、fw_payload.bin都编译出来。
- SOC制造商可以选择：
  - 使用一个OpenSBI参考固件作为它们的M模式**运行时**固件
  - 把OpenSBI作为库从零构建M模式**运行时**固件
  - 把OpenSBI作为库的现存M模式固件(U-Boot_M_mode/EDK2)

##### FW_PAYLOAD

- 把下个启动阶段作为payload的OpenSBI固件
  - 任意S模式下的BOOTLOADER/OS镜像作为OpenSBI FW_PAYLOAD的payload
  - 允许覆盖设备树blob(如DTB)
  - 非常类似于BBL，所以可以很好地适应SiFive Unleashed board的现有启动流程
- 缺点
  - 一旦OpenSBI或BOOTLOADER改变，就必须重新生成FW_PAYLOAD镜像
  - 无法从之前的启动阶段(LOADER)传递参数给FW_PAYLOAD
- 配置选项
  + **FW_PAYLOAD_OFFSET** - 基于FW_TEXT_BASE的位移，payload二进制将从FW_TEXT_OFFSET链接到最终的FW_PAYLOAD固件二进制镜像。如没有定义FW_PAYLOAD_ALIGN则此参数必须使用。如果错误定义了FW_PAYLOAD_OFFSET或FW_PAYLOAD_ALIGN，或者两者都没有定义，则会产生编译错误。
  + **FW_PAYLOAD_ALIGN** - 地址对齐约束，payload二进制将被链接在基本固件二进制的后面，从而形成最终的FW_PAYLOAD固件二进制镜像。如没有定义FW_PAYLOAD_OFFSET则此参数必须使用。如果同时定义了FW_PAYLOAD_OFFSET或FW_PAYLOAD_ALIGN，则会使用FW_PAYLOAD_OFFSET而忽略FW_PAYLOAD_ALIGN。
  + **FW_PAYLOAD_PATH** 镜像文件的路径。如没有指定此参数，则会自动生成一个简单的测试payload。它在打印出一条信息后就进到一个无限循环里。
  + **FW_PAYLOAD_FDT_PATH** 到FDT二进制文件的路径，它最终会嵌入到固件二进制的`.text` 段里。如果没有定义这个选项，且平台也没有定义它的文件（参考FW_PAYLOAD_FDT），则固件会期望上一个启动阶段来把FDT作为参数传递过来。
  + **FW_PAYLOAD_FDT** 平台代码自己定义的FDT的路径。文件名必须满足平台`objects.mk`文件里 DTB文件的名称(platform-dtb-y条目)。此选项将会导致FW_PAYLOAD_FDT_PATH被自动设置。在 `make` 命令行里定义FW_PAYLOAD_FDT_PATH将会使此选项失效，命令行里定义的那个DTB文件将会构建到最终的固件里。
  + **FW_PAYLOAD_FDT_ADDR** 在进行下一个启动阶段之前，FDT的放置地址。这个FDT可能是上个启动阶段传递的，也可能是FW_PAYLOAD_FDT_PATH定义的，它会嵌入到`.text`段里。如果没有提供这个参数，固件会把上个阶段传递的FDT地址直接传递给下个阶段。

##### FW_JUMP

- 用固定跳转地址指向下个启动阶段的OpenSBI固件
  - 下个启动阶段(BOOTLOADER)和FW_JUMP都是由上个启动阶段(LOADER)装载的
  - 对于QEMU来说非常有用，因为我们可以使用预编译的FW_JUMP
- 缺点
  - 上个启动阶段(LOADER)必须把下个启动阶段(BOOTLOADER)加载到固定位置
  - 无法从之前的启动阶段(LOADER)传递参数给FW_JUMP

##### FW_DYNAMIC

- OpenSBI带有下个启动阶段的动态信息
  - 下个启动阶段(BOOTLOADER)和FW_DYNAMIC都是由上个启动阶段(LOADER)装载的
  - 通过a2寄存器，上个启动阶段(LOADER)把`struct fw_dynamic_info`的位置传递给FW_DYNAMIC
- 缺点
  - 上个启动阶段(LOADER)需要知道`struct fw_dynamic_info`

##### 作为库来使用

- 外部的M模式的固件链接到OpenSBI库(如EDK2的OpenSBI集成)

##### 使用OpenSBI库的限制

- 外部的固件和OpenSBI源码使用相同的GCC target选项(如-march, -mabi, -mcmodel)

- 外部的固件必须创建每个核不重叠

- 从外部固件调用OpenSBI函数的两个限制

  - RISC-V的mscratch寄存器必须指向一个有效的OpenSBI scratch space(如一个sbi_scratch结构体)

  - RISC-V的sp寄存器必须设置为使每个核都指向不重叠的栈

- 外部固件必须确保：

  当调用`sbi_init()`的时候中断是关闭的

  每个调用`sbi_init()`的核是powered-up的，或在响应CPU hotplug事件

  `sbi_trap_handler()`为M模式下的中断或自陷而调用

##### 两种静态链接库

1. libsbi.a - 平台无关，实现了SBI规范的接口，由于不包含平台相关的部分，所以必须实现与平台相关的钩子且链接到本库里。

   平台支持是通过数据结构`sbi_platform`来实现的，所以必须要提供钩子`sbi_platform`的实现。

2. libplatsbi.a - 即包括了libsbi.a，也包括了平台相关的钩子。

从调用者的角度来看最重要的函数是`sbi_init`和`sbi_trap_handler`。在调用`sbi_init`和`sbi_trap_handler`之前，mstatus和mie寄存器里必须设置中断是无效的。每个核都应该调用一遍`sbi_init`函数。`sbi_trap_handler`函数用以处理如下中断

- M态计时器中断
- Ｍ态软中断
- 非法指令异常
- 未对齐load异常
- 未对齐store异常
- Ｓ态ecall异常
- H态ecall异常



#### 编译

##### libsbi.a

```
export CROSS_COMPILE=riscv64-unknown-elf-	# 交叉编译必须定义环境变量
make	# 在build/lib目录可找到编译生成的libsbi.a
make install	# 创建install目录，供其它项目使用
```

##### libplatsbi.a

```
make PLATFORM=kendryte/k210		# 在build/platform/kendryte/k210/目录下，libplatsbi.a在lib目录，固件在firmware目录
make PLATFORM=kendryte/k210 intall	# 创建install/platform/kendryte/k210/目录，会把对应的lib目录和firmware目录复制过来
```

##### OpenSBI镜像

一般来说，生成的镜像是32位的还是64位的，取决于提供的交叉编译工具链。但也可以通过参数`PLATFORM_RISCV_XLEN`来指定所需的宽度。

#### 源码分析

以generic平台payload模式为例。

##### 汇编部分

```
_start: 确定启动核的id，这要靠_try_lottery
_try_lottery: 第一个访问此标记的核被判定为启动核,启动核把实际装载地址_fw_start保存进变量_load_start，向下执行到relocate；非启动核则执行_wait_relocate_copy_done等待启动核完成所有工作。
_relocate: 如链接位置和装载位置一致，则跳转到_relocate_done，表示完成了装载；否则用接下来的四个标记完成装载，然后跳转到_relocate_done。
_relocate_copy_to_lower:
_relocate_copy_to_lower_loop:
_relocate_copy_to_upper:
_relocate_copy_to_upper_loop:
_wait_relocate_copy_done: 仅有非启动核才执行到此处，仅仅是跳转到_wait_for_boot_hart
_relocate_done: 将变量_boot_status置1，表示完成装载。然后重置寄存器，向下执行到_bss_zero。
_bss_zero:
_scratch_init:
_fdt_reloc_again:
_fdt_reloc_done:
_wait_for_boot_hart: 非启动核循环等待，仅有启动核将变量_boot_status置为2，才向下执行_start_warm
_start_warm: 对所有核设置寄存器，然后跳转到C的部分sbi_init。
_skip_trap_handler_rv32_hyp: 仅用于32位机器
_skip_trap_exit_rv32_hyp: 仅用于32位机器
# 以下部分是变量、函数、宏的定义，正常的代码流不应到此。
_runtime_offset:
_relocate_lottery:
_boot_status:
_load_start:
_link_start:
_link_end:
_hartid_to_scratch:
_start_hang:
_trap_handler:
_trap_exit:
_trap_handler_rv32_hyp:
_trap_exit_rv32_hyp:
_reset_regs:
```



从`./firmware/fw_payload.elf.ldS`可见，opensbi从`_start`开始执行，位置在`firmware/fw_base.S`。

`MOV_3R`用于保存`a0,a1,a2,a3`这三个寄存器的值，这是为下一条指令`call fw_boot_hart`做准备，因为`fw_boot_hart`要用到这个三寄存器。

`fw_boot_hart`的作用是返回当前核的`Hart ID`。对payload模式，实际上`a0`返回的值是`-1`，这是为了在后面的代码里执行`_try_lottery`选出一个启动核。

接下来把`a0`的值保存在`a6`里，然后恢复`a0,a1,a2`的值。如`Hart ID`为`-1`，说明需要挑选出一个boot hart，则执行`_try_lottery`；如`Hart ID`不为`a0`，说明当前核不是boot hart，则执行`_wait_reloate_copy_done`。

`_relocate_lottery`就是一个变量，初值为0，每当一个核启动这个变量值就加1。当第一个核启动的时候，得到初值为0，于是被选为启动核；当其它核启动的时候，初值非0，则执行`_wait_reloate_copy_done`。

代码重定位：判断`_load_start`与`_start`是否一致。若不一致，则会将代码重定位。

清除通用寄存器的值：但a1和a2不会清除。`call _reset_regs`

清除bss段。`_bss_zero`，`_bss_start`和`_bss_end`是bss段的起始地址和结束地址，用一个循环把这段内存置0.

设置临时的trap处理例程。其实就是写mtvec寄存器，而这个处理例程`_start_hang`就是个死循环。

设置临时的栈。即设置sp寄存器的值，这段栈就在bss段的末尾。

让固件保存信息。即调用`fw_save_info`，由于payload模式不需要保存信息，故直接执行`ret`。

初始化平台。即执行`call fw_platform_init`读取设备树里的信息。此函数在`platform/generic/platform.c`。a0传递参数0，a1传递设备树地址，a2传递设备树大小，a3传递0。

重新加载核的信息。即`hart_count`和`hart_stack_size`，它们都定义在结构体`struct sbi_platform`里，它们的值由前面的`fw_platform_init`函数确定。

设置当前核的暂存空间。它被放在`_fw_end`的后面。

把当前核暂存空间的地址放在tp寄存器。

把结构体`struct sbi_scratch`的信息保存进当前核的暂存空间。

- fw_start是固件的开始地址，即_fw_start的地址.
- fw_size是固件大小，即_fw_end - _fw_start。
- next_arg1是当前核下个启动阶段的arg1，或a1寄存器的值，使用`fw_next_arg1`函数可得。
- next_addr是当前核下个启动阶段的地址，通过`fw_next_addr`函数可得。
- next_mode是当前核下个启动阶段的特权级，即S模式。
- warmboot_addr是当前核热启动的入口，即`_start_warm`的地址。
- platform_addr是sbi_platform的入口，即结构体`platform`的地址。
- hartid_to_scratch是hartid到sbi_scratch转换函数的地址，即`_hartid_to_scratch`函数。
- trap_exit是trap退出函数的地址，即`_hartid_to_scratch`函数。
- tmp0是临时存储，目前是清空该值。
- options是OpenSBI库的选项，如果从参数`FW_OPTIONS`传入则为`FW_OPTIONS`的值，否则通过函数`fw_options`获取，对payload模式这个值为0。

fdt重定位。a1保存了源fdt的地址，通过`fw_next_arg1`函数可得到目的fdt的地址。如果a1为0、或a1等于目的fdt的地址、或目的fdt为0，无需重定位，直接跳转到`_fdt_reloc_done`；否则计算出fdt的大小，在一个循环里完成fdt的重载。

将`_boot_status`的值改为2，告诉非启动核们已经完成了启动。

所有核都会执行`_start_warm`处的内容。

1. 调用`_reset_regs`函数把常用的通用寄存器清零。
2. 把mip和mie寄存器清0
3. 获取结构体`sbi_platform`里的`hart_count`、`hart_stack_size`、`hart_index2id`，以及`mhartid`寄存器的值。
4. 如果`hart_index2id`的值为0则跳转到3f，否则将向下执行标记1，不过在此之前把计数器a4置0。
5. 标记1：通过循环遍历`hart_index2id`所指向的数组，如果某个数组元素与mhartid的值相等则跳转到2f，此时a4的值即为那个数组元素的索引。否则，数组的所有元素都不等于mhartid，则此a4将取-1。然后向下执行标记2.
6. 标记2：把a4的值保存进s6。
7. 标记3：如果s6大于s7，则表明数组`hart_index2id`里没有一个元素匹配当前的hart id，跳转到`_start_hang`进入死循环。当前核的暂存空间在当前核栈空间的顶部，基于此找到当前的暂存空间。然后把暂存空间的地址写入mscratch和sp两个寄存器。
8. 把`_trap_handler`的地址放入mtvec寄存器，即设置中断处理例程。

执行`call sbi_init`跳转到`sbi_init`，传递的参数即为结构体`sbi_scratch`的指针。至此，汇编部分就结束了。

##### C部分

sbi_init : 首先判断是否支持下个启动阶段的特权级，如果不支持则执行`sbi_hart_hang()`函数进入低功耗模式且无限循环；如果支持则通过`atomic_xchg()`选出执行冷启动的核(其实就是第一个访问此函数的核)，此核会执行`init_coldboot()`，其它核则执行`init_warmboot()`。

init_coldboot : 

- sbi_scratch_init - 定义在`lib/sbi/sbi_scratch.c`，作用是初始化暂存空间的列表和分配器，即数组`hartid_to_scratch_table`。这是首先要作的事。

- **sbi_domain_init** - 初始化动态加载的镜像的模块
- **sbi_platform_early_init** - 平台的早期初始化
- **sbi_console_init** - 控制台初始化，从这里开始，就可以使用串口输出了。
- **sbi_platform_irqchip_init** - irq中断初始化
- **sbi_ipi_init** - 核间中断初始化
- **sbi_tlb_init** - mmu的tlb表的初始化
- **sbi_timer_init** - timer初始化
- **sbi_hsm_prepare_next_jump** - 准备下一级的boot

init_warmboot：

- wait_for_coldboot
- sbi_hsm_hart_get_state
- sbi_hart_switch_mode
