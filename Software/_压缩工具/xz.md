#### 简介

压缩或解压的工具。语法类似于gzip和bzip2。原生支持xz格式，也支持zlma格式和原始压缩流。

压缩的时候会自动生成带xz后缀的文件，解压的时候会自动生成去掉xz后缀的文件。

#### 语法

```
xz [option] [file...]
unxz	# 等价于xz -d，解压xz文件
zxcat	# 等价于xz -dc，解压xz文件并输出到stdout
lzma	# 等价于xz -F lzma，压缩为lzma格式
unlzma	# 等价于xz -d -F lzma，解压lzma文件
lzcat	# 等价于xz -dc -F lzma，解压lzma文件并输出到stdout
```

#### 选项

##### 操作模式

```
-z, --compress	# 压缩
-d, --decompress, --uncompress	# 解压
-t, --test
-l, --list
```

##### 操作修饰符

```
-c, --stdout, --to-stdout	# 把压缩或解压的数据写到标准输出。如不使用此选项则输出到文件。
-f
--file[=file]
--file0[=file]
-k, --keep	# 不删除输入文件
--no-sparse
--single-steam
-S <.suf>

```

##### 基本文件格式与压缩选项

```
-0 ... -9
--block-list=<sizes>
--block-size=<size>
-C <check>
-e
--fast
--flush-timeout=<timout>
-F, --format=<fmt>	# 定义压缩或解压的格式。
	# <fmt>可能的值：
	# auto：默认的值。压缩时为xz，解压时会自动检测。但原始流无法被自动检测。
	# xz：压缩或解压均为xz格式。
	# lzma, alone: 压缩或解压均为lzma格式。
	# raw: 压缩或解压原始流。
--ignore-check
--memlimit-compress=<limit>
--memlimit-decompress=<limit>
-M <limit>
--no-adjust
-T <threads>
```

##### 配置压缩过滤链

##### 其它

#### 示例

##### 基本

```
xz foo	# 把文件foo压缩为foo.xz，并在压缩成功后删除foo
xz -dk bar.xz	# 解压缩bar.xz为bar，且在解压成功后不删除bar.xz
tar cf - baz | xz -4e > baz.tar.xz
	# 从文件baz创建baz.tar.xz，比默认的-6更慢但会需要更少的内存
xz -dcf a.txt b.txt.xz c.txt d.txt.lzma > abcd.txt
	# 只用一条命令就可以把压缩文件和非压缩文件合并为一个文件
```

##### 并行压缩

##### 机器人模式

##### 配置压缩过滤链
