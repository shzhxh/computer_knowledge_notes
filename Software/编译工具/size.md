#### 简介

```
size [options] [objfile...]	# 列出一程序段的大小和总的大小。
	# objfile... : 待检测的目标文件。如不指定，则默认为a.out。
```

#### 选项

```
-A, --format=sysv	# 类似于System V的输出。
-B, --format=berkeley	# 类似于Berkeley的输出。默认为此输出。
-d, --radix=10	# 以10进制输出
-o, --radix=8	# 以8进制输出
-x, --radix=16	# 以16进制输出
--common	# 打印每个文件中常用符号的总大小。当使用Berkeley格式时，这些都包含在bss大小中。
-t, --totals
--target=<bfdname>
-V, --version
```

