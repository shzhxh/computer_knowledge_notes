#### 简介

OpenSBI是运行在M态的固件，用以支持H态或S态的程序，如hypervisor、bootloader或通用操作系统。

#### OpenSBI库用法

OpenSBI提供了两种静态链接库：

1. libsbi.a - 平台无关，实现了SBI规范的接口，由于不包含平台相关的部分，所以必须实现与平台相关的钩子且链接到本库里。

   平台支持是通过数据结构`sbi_platform`来实现的，所以必须要提供钩子`sbi_platform`的实现。

2. libplatsbi.a - 即包括了libsbi.a，也包括了平台相关的钩子。

使用OpenSBI静态链接库的软件需要使用用OpenSBI相同的编译选项

- -mabi, -march, -mcmodel

调用OpenSBI库函数的约束

1. RISC-V的mscratch寄存器必须指向一个有效的OpenSBI scratch space(如一个sbi_scratch结构体)
2. RISC-V的sp寄存器必须设置为使每个核都指向不重叠的栈

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

#### 添加平台支持

通过实现结构体`sbi_platform`来定义平台相关的钩子，这样libsbi.a就可以进行平台相关的操作了。

OpenSBI也提供了可启动的运行时固件，那些固件链接到了libplatsbi.a，保存在install/platform/<platform_subdir>/bin目录下。那些固件用以替代BBL。可以在include/sbi/sbi_platform.h读到关于`sbi_platform`和相关APIs的完整信息。

