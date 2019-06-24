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
-f
-p
-P
-h
-x
-d, --disassemble		# 显示代码段的汇编内容
-D, --disassemble-all	# 显示所有段的汇编内容
-S, --source			# 汇编码与源码混合显示
-s, --full-contents		# 显示所有段的内容
-g
-e
-G
-W[lLiaprmfFsoRtUuTgAckK]
-t, --syms				# 显示符号表的内容
-T
-r
-R
```

##### 可选开关

```
-C			# 逆向解析C++符号名
-l			# 汇编码中插入符号名和行号
-j section	# 仅反汇编指定的section
```



#### 用法

```shell
objdump -S testElf > testElf.asm	# 将testElf反汇编至testElf.asm,源码也将展现
objdump -t bin/kernel > kernel.sym	# 将kernel的符号表输出到文件kernel.sym中
objdump -h bin/kernel			# 显示kernel的所有section headers的信息
```

