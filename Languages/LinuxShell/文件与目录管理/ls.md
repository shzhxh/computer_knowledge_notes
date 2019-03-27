#### 语法

```
ls [选项] [文件]	# 列出文件的信息，默认以字母序排列
```

#### 选项

```
-a, --all		# 列出所有文件，包括 . 和 ..
-A, --almost-all	# 列出所有文件，但不包括 . 和 ..
-b,
-B,
-c
-C
-d
-D
-f
-F
-g
-G
-h, --human-readable	# 以人类可读的方式展示文件大小
-H
-i, --inode				# 打印出每个文件的索引号
-I, --ignore=PATTERN	# 不显示满足PATTERN的条目
-k
-l		# 显示详细信息
-L
-m
-n
-N
-o
-p
-q
-Q
-r
-R, --recursive		# 递归地列出所有文件
-s
-t
-T
-u
-U
-v
-w
-x
-X
-Z
-1			# 每行只列出一个文件
--help
--version
```



#### 用法示例

```shell
ls -lR | grep "^-" | wc -l		# 递归统计当前目录下所有文件的个数
ls -lR | grep "^d" | wc -l		#递归统计当前目录下所有文件夹的个数
ls -d .*				#显示当前目录下的隐藏文件
ls -hl .	# 以人类可读的方式显示文件大小
```

