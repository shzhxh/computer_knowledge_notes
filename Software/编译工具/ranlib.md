#### 作用
为archieve生成索引并把它存储在archieve中。这个索引可以加快链接到库的速度，并允许库中的例程相互调用而不必考虑它们在库中的位置。可以使用`nm -s`或`nm -print-armap`列出该索引。`ranlib`程序和运行`ar -s`命令是完全等价的。
#### 语法
- ranlib [--plugin name][-DhHvVt] archive

#### rv64版

```
riscv64-unknown-elf-ranlib [options] archive
@<file>		# 从file中读出选项
--plugin name	# 载入指定plugin
-D
-U
-t
-h --help
-v --version
# 支持的targets
elf64-littleriscv elf32-littleriscv elf64-little elf64-big elf32-little elf32-big plugin srec symbolsrec verilog tekhex binary ihex
```



