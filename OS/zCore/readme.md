#### 编译过程

```bash
make rv64-image	# 从Makefile可见，是使用rcore-fs-fuse命令创建文件系统riscv64.img，文件系统的内容来自于rootfs目录。
cd zCore && make build linux=1 arch=riscv64	# 从Makefile可见，最终执行的是cargo build $(build_args)
	# $(build_args)默认自带-Z build-std=core,alloc --target $(arch).json，还会附加其它参数，这是由make build的参数引入的，比如linux=1会引入--features linux，比如arch=riscv64且没有指定board的值会引入--features board_qemu等
	# $(arch).json指定了链接脚本，对于riscv64它是linker64.ld，对于x86_64它是linker.ld，对于aarch64它是linker.ld，在链接脚本里指定内核入口点为_start
```

#### 执行过程

##### _start

对于riscv64，入口点`_start`定义在`entry.rs`，然后跳转到`primary_rust_main`进到rust代码里执行，`primary_rust_main`再调用`primary_main`。

对于aarch64，入口点`_start`定义在`entry.rs`，它的下一个跳转点是`rust_main`进到rust代码里执行，`rust_main`再调用`primary_main`。

对于x86_64，入口点`_start`定义在`entry.rs`，它准备好相关参数，最后跳转到`primary_main`。

##### rust_main

`rust_main`仅针对riscv64平台，位置在`entry.rs`。它的两个参数是来自于opensbi，由`fw_platform_init`函数设置`a0,a1`两个寄存器得到的。

`rust_main`准备好相关参数后，调用`primary_main`开始执行。

##### primary_main

`primary_main`初始化日志系统，可以打印字符。初始化内存，重新映射内核。最后执行`run`函数开始调度用户态进程。

#### 库之间的依赖关系

##### zCore的依赖

- kernel-hal -> zcore-drivers(**drivers**)

- zcore-loader(**loader**) -> kernel-hal

  -> zircon-object

  -> linux-object

  -> zircon-syscall

  -> linux-syscall

- zircon-object -> kernel-hal

- linux-object -> zircon-object

  -> kernel-hel

  -> zircon-driver(**drivers**)

linux-syscall -> zircon-object

-> linux-object

-> kernel-hal

zircon-syscall-> zircon-object

-> kernel-hal

##### zCore的结构

| 层级 | 名字                          | 作用                                   |
| ---- | ----------------------------- | -------------------------------------- |
| 6    | zCore                         | 顶层的库，调用底层库提供的各种功能     |
| 5    | loader                        | 装载和执行用户程序，支持上层调用下层   |
| 4    | linux-syscall, zircon-syscall | 实现系统调用，支持上层调用下层         |
| 3    | linux-object, zircon-object   | 实现内核对象，支持上层调用下层         |
| 2    | kernel-hal                    | 实现硬件抽象层的接口，支持上层调用下层 |
| 1    | drivers                       | 实现驱动程序，对上层提供支持           |

