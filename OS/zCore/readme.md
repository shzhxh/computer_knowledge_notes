#### 1

`make rv64-image`

使用`rcore-fs-fuse`命令创建文件系统riscv64.img，文件系统的内容来自于rootfs目录。

#### 2

`make run linux=1 arch=riscv64`

依赖于`build`和`justrun`。`build`先执行了`kernel`，后执行了`kernel_bin`。

`build`的参数有：`-Z build-std=core,alloc --target $(arch).json`，这是默认自带的；`--features linux`，这是`linux=1`引入的。

`arch=riscv64`则规定了`build_args, build_path, qemu, OBJDUMP, OBJCOPY, ARCH, qemu_opts`。

#### 3

`build`的参数`rsicv.json`里指定了链接的方式，即`lnker64.ld`。它说入口点为`_start`。

#### 4

对于riscv64，入口点`_start`在`entry64.asm`。它设置`satp`寄存器以映射虚拟地址，然后在虚拟空间里设置`sp`寄存器，跳转到`rust_main`。

#### 5

`rust_main`在`main.rs`。初始化日志系统，可以打印字符。初始化内存，重新映射内核。然后跳转到`main`开始执行。