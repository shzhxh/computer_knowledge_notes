```
cpio -i [options] [patterns] [< archive]	# 从archive复制到文件夹
cpio -o [options] < name-list [> archive]	# 从文件夹复制到archive
cpio -p [options] dest-dir < name-list		# 
```

#### 操作模式

cpio的参数里必须带如下模式之一：

```
-i, --extract	# 从archive解压文件，运行于复制进模式。从标准输入读取archive并解压出来。
-o, --create	# 创建archive，运行于复制出模式。从标准输入读取文件名列表并输出到标准输出。
-p, --pass-through	# 运行于复制通过模式，从标准输入读取文件名列表并把它们复制到指定目录。
-?, --help
--usage
--version
```



#### 适用于所有模式

```
-B	# 设置I/O块大小为5120字节。
-c	# 使用老的可移植archive格式(ASCII)。等价于"-H odc"。
-H, --format=FORMAT	# 指定archive的格式为FORMAT，可选FORMAT如下：
	bin		# 过时的二进制格式
	odc		# 老的可移植格式(POSIX.1)
	newc	# 新的可移植格式(SVR4)
	crc		# 新的可移植格式，且附带checksum(SVR4)
	tar		# 老的tar格式
	ustar	# tar格式(POSIX.1)
	hpbin	# 过时的二进制格式(适用于HPUX的cpio)
	hpodc	# 可移植格式(适用于HPUX的cpio)
-v, --verbose	# 详细模式。
```



#### 适用于复制进和复制出模式

```
-F, --file=[[USER@]HOST:]FILE-NAME	# 用FILE-NAME来代替标准输入或标准输出，USER和HOST用于远程archive的情况
```



#### 仅适用于复制进模式

#### 仅适用于复制出模式

#### 仅适用于复制通过模式

#### 适用于复制出和复制通过模式

#### 适用于复制进和复制通过模式

