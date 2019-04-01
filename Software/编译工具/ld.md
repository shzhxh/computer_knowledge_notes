#### 简介

把多个object合成一个。

#### 用法(riscv64版本)

```
riscv64-unknown-elf-ld [options] file...
```

- **supported targets**

   elf64-littleriscv elf32-littleriscv elf64-little elf64-big elf32-little elf32-big plugin srec symbolsrec verilog tekhex binary ihex

- **supported emulations**

  elf64lriscv elf32lriscv

#### 选项

```
-A ARCH, --architecture ARCH	# 设置architecture
-b TARGET, --format TARGET  # 指定接下来输入文件的target
-m EMULATION		# 设置emulation，可以用-V或--verbose列出所有的emulation。
-v, --version		# 打印版本号。
-V					# 除了打印版本号，还列出所支持的emulations。
--dll-verbose, --verbose		# 除了-V的功能，还会打印出连接器将要使用的链接脚本。
```

