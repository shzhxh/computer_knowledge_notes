#### 简介

显示object文件的信息。(参考：man objdump)

#### 语法

```
objdump [options] objfile...	# 显示objfile的信息，options控制显示的内容。
```

#### 选项

##### 必选开关

```
-a
-d, --disassemble		# 显示代码段的汇编内容
-D, --disassemble-all	# 显示所有段的汇编内容
-e
-f						# 显示头文件信息
-g
-G
-h						# 显示所有section header的信息
-H, --help
-p
-P
-r
-R
-s, --full-contents		# 显示所有段的内容
-S, --source			# 汇编码与源码混合显示
-t, --syms				# 显示符号表的内容
-T
-V
-x						# 显示所有header的信息

-W[lLiaprmfFsoRtUuTgAckK]
```

##### 可选开关

```
-b, --target=bfdname	# 指定目标文件的格式为bfdname
-C			# 逆向解析C++符号名
-j section	# 仅反汇编指定的section
-l			# 汇编码中插入符号名和行号
-m, --architecture=machine	# 指定目标架构为machine
```



#### 用法

```shell
objdump -S testElf > testElf.asm	# 将testElf反汇编至testElf.asm,源码也将展现
objdump -t bin/kernel > kernel.sym	# 将kernel的符号表输出到文件kernel.sym中
objdump -h bin/kernel				# 显示kernel的所有section headers的信息
objdump -D -b binary -m riscv out.bin	# 反汇编二进制文件out.bin
```

