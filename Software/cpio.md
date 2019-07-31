```
cpio [options] [dir]	# dir和archives之间复制文件
```

主操作模式

```
-i, --extract	# 从archive解压文件，运行于复制进模式
-o, --create	# 创建archive，运行于复制出模式
-p, --pass-through	# 运行于复制通过模式
-t, --list		# 列出input的内容
```



通用选项

```
-H, --format=FORMAT	# 指定archive的格式为FORMAT，FORMAT详见man cpio
```



适用于复制进和复制出模式

```
-F, --file=[[USER@]HOST:]FILE-NAME	# 用FILE-NAME来代替标准输入或标准输出，USER和HOST用于远程archive的情况
```



仅适用于复制进模式

仅适用于复制出模式

仅适用于复制通过模式

适用于复制出和复制通过模式

适用于复制进和复制通过模式

