#### 简介

解压zip文件的工具

#### 语法

```
unzip [命令[选项]]　file[.zip] [file...] [-x xfile...] [-d exdir]
```

#### 命令

```
-p
-f
-u
-v
-x xfiles	# 不包含xfiles里的文件
-l			# 列出文件
-t
-z
-T
-d exdir	# 将文件解压到exdir
```

#### 选项

```
-n
-o			# 不提示而直接覆盖文件
-j
-U
-C
-X
-K
-q			# 静默模式
-a
-aa
-UU
-L
-V
-M
-O CHARSET	# 为DOS,Windows或OS/2的zip文件指定字符编码
-I CHARSET	# 为UNIX的zip文件指定字符编码
```



#### 示例

```
unzip -l "Weibo Data.zip"	# 显示"weibo Data.zip"所包含的文件
unzip "Weibo Data.zip"		# 解压"Weibo Data.zip"到当前文件夹
unzip src.zip -d /usr/src/	# 解压src.zip到目录/usr/src/
unzip -O cp936				# cp936即GBK，因在Code Page的第936页而得名
```

