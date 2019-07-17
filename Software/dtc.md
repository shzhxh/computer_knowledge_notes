#### 简介

 Device Tree Compiler，输入指定格式的设备树，输出可以启动内核的设备树。一般来说，输入的是人类可读的dts格式，而输出的是dtb格式或二进制格式。

#### 用法

```bash
dtc [options] <input file>
```

- `-I <input format>`指定输入文件的格式，可选的格式有：dts(device tree source text), dtb( device tree blob), fs( /proc/device-tree style directory).
- `-O <output format>`指定输出文件的格式，可选的格式有：dts, dtb, asm(assembler source).
- `-o <output file>`将结果输出到文件，而不是stdout。