#### 运行xv6

```
# 目标：xv6 for riscv (Ubuntu 18.04)

# 准备
sudo apt install gcc-8-riscv64-linux-gnu	# Install toolchain
# 需要自行编译qemu，编译目标为riscv64-softmmu

# 运行
git clone git://github.com/mit-pdos/xv6-riscv.git
cd xv6-riscv
vim Makefile	# 需要将"gcc"改为"gcc-8"
make qemu
```

#### kernel.img(对于x86)

1. bootasm.S和bootmain.c编译为bootblock
2. bio.c, console.c, exec.c, file.c, fs.c, ide.c, ioapic.c, kalloc.c, kbd.c, lapic.c, log.c, main.c, mp.c, picirq.c, pipe.c, proc.c, sleeplock.c, spinlock.c, string.c, swtch.S, syscall.c, sysfile.c, sysproc.c, trapasm.S, trap.c, uart.c, vectors.pl, vm.c, entry.Sentryother.S, initcode.S编译为kernel
3. 将boolblock和kernel装入kernel.img里

#### kernel(对于riscv)

将entry.S, start.c, console.c, printf.c, uart.c, kalloc,c, spinlock,c, string.c, main.c, vm.c, proc.c, swtch.S, trampline.S, trap.c, syscall.c, sysproc.c, bio.c, fs.c, log.c, sleeplock.c, file.c, pipe.c, exec.c, sysfile.c, kernelvec.S, plic.c, virtio_disk.c编译为kernel

#### fs.img(对于x86和riscv)

1. 使用ulib.c, usys.S, printf.c, umalloc.c做为用户库。
2. 使用以上库，将cat.c, echo.c, forktest.c, grep.c, init.c, kill.c, ln.c, ls.c, mkdir.c, rm.c, sh.c, streefs.c, usertests.c, wc.c, zombie.c编译为对应的应用程序。
3. 使用mkfs将以上应用程序放到一个文件系统里，即为fs.img。