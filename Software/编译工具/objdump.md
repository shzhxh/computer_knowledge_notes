#### 简介

显示object文件的信息。(参考：man objdump)

#### 语法

```
objdump [options] objfile...	# 显示objfile的信息，options控制显示的内容。
```

#### 选项

```

```



#### 用法

```shell
objdump -S testElf > testElf.asm	# 将testElf反汇编至testElf.asm,源码也将展现
objdump -t bin/kernel > kernel.sym	# 将kernel的符号表输出到文件kernel.sym中
objdump -h bin/kernel			# 显示kernel的所有section headers的信息
```

