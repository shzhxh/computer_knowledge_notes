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
  + **FW_PAYLOAD_FDT_PATH** 到FDT二进制文件的路径，它最终会嵌入到固件二进制的*.text* 段里。如果没有定义这个选项，且平台也没有定义它的文件（参考FW_PAYLOAD_FDT），则固件会期望上一个启动阶段来把FDT作为参数传递过来。
  + **FW_PAYLOAD_FDT** 平台代码自己定义的FDT的路径。文件名必须满足平台*objects.mk*文件里 DTB文件的名称(*platform-dtb-y*条目)。此选项将会导致*FW_PAYLOAD_FDT_PATH*被自动设置。在 `make` 命令行里定义FW_PAYLOAD_FDT_PATH将会使此选项失效，命令行里定义的那个DTB文件将会构建到最终的固件里。
  + **FW_PAYLOAD_FDT_ADDR** 在进行下一个启动阶段之前，FDT的放置地址。这个FDT可能是上个启动阶段传递的，也可能是FW_PAYLOAD_FDT_PATH定义的，它会嵌入到*.text*段里。如果没有提供这个参数，固件会把上个阶段传递的FDT地址直接传递给下个阶段。

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

opensbi从`_start`开始执行，位置在`firmware/fw_base.S`。`MOV_3R`用于保存`a0,a1,a2,a3`这三个寄存器的值，这是为下一条指令`call fw_boot_hart`做准备，因为`fw_boot_hart`要用到这个三寄存器。

`fw_boot_hart`的作用是返回当前核的`Hart ID`。它前期进行了两个判断，一是魔数要匹配，二是版本号不能高于最高值，任意一个条件不满足，都会进到死循环`_bad_dynamic_info`里。`Hart ID`保存在`a0`里，然后函数返回。实际上`a0`返回的值是`-1`，**不理解这一点**。

接下来把`a0`的值保存在`a6`里，然后恢复`a0,a1,a2`的值。如`Hart ID`为`-1`，说明需要挑选出一个boot hart，则执行`_try_lottery`；如`Hart ID`不为`a0`，说明当前核不是boot hart，则执行`_wait_reloate_copy_done`。

