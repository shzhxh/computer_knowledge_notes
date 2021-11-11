#### 编译过程

```bash
make rv64-image	# 从Makefile可见，是使用rcore-fs-fuse命令创建文件系统riscv64.img，文件系统的内容来自于rootfs目录。
cd zCore && make build linux=1 arch=riscv64	# 从Makefile可见，最终执行的是cargo build $(build_args)
	# $(build_args)默认自带-Z build-std=core,alloc --target $(arch).json，还会附加其它参数，这是由make build的参数引入的，比如linux=1会引入--features linux，比如arch=riscv64且没有指定board的值会引入--features board_qemu等
	# $(arch).json指定了链接脚本，对于riscv64它是linker64.ld，对于x86_64它是linker.ld，在链接脚本里指定内核入口点为_start
```

#### 执行过程

##### _start

对于riscv64，入口点`_start`定义在`entry64.asm`，通过`global_asm!`把这段汇编代码嵌入到`main.rs`。从`_start`开始先设置`satp`寄存器以映射虚拟地址，然后在虚拟空间里设置`sp`寄存器，跳转到`rust_main`。(`rust_main`仅有一个，是针对riscv64平台的)

对于x86_64，入口点`_start`定义在`main.rs`，它最后跳转到`main`。(`main`有两个，是针对linux和zircon两种情况)

##### rust_main

`rust_main`仅针对riscv64平台，位置在`main.rs`。它的两个参数是来自于opensbi，由`fw_platform_init`函数设置`a0,a1`两个寄存器得到的。

`rust_main`初始化日志系统，可以打印字符。初始化内存，重新映射内核。然后跳转到`main`开始执行。(`main`有两个，是针对linux和zircon两种情况)

##### main

`main`最后执行`run`函数开始调度用户态进程。