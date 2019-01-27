#### 用法(riscv64版本)

```
riscv64-unknown-elf-ld [options] file...
-A ARCH, --architecture ARCH	# 设置architecture
-b TARGET, --format TARGET  # 指定接下来输入文件的target
-m EMULATION		# 设置emulation
```

- **supported targets**

   elf64-littleriscv elf32-littleriscv elf64-little elf64-big elf32-little elf32-big plugin srec symbolsrec verilog tekhex binary ihex

- **supported emulations**

  elf64lriscv elf32lriscv