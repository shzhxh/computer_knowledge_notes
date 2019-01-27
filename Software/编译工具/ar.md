#### 作用
创建或修改库，或从库中提取文件。**archive**是一个单独的文件，是其它文件的集合。**member**是archive里的文件。
#### 语法

```shell
ar [-X32_64][-]p[mod][--plugin name] [--target bfdname][relpos] [count] archive [member...]
# r：向库中插入文件，如果存在则覆盖。
# c：创建一个库。
# v：显示详细信息。
# s: 为archieve创建或更新一个索引。注意：s选项只能单独使用。
```

#### rv64版

```
riscv64-unknown-elf-ar [emulation options] [-]{dmpqrstx}[abcDfilMNoPsSTuvV] [--plugin <name>] [member-name] [count] archive-file file...
# emulation options：无
# commands
# command specific modifiers
# generic modifiers
  --target=BFDNAME	# 将目标格式指定为BFDNAME，支持的目标有elf64-littleriscv elf32-littleriscv elf64-little elf64-big elf32-little elf32-big plugin srec symbolsrec verilog tekhex binary ihex
# optional
  --plugin <p>	# 载入指定的plugin
```

