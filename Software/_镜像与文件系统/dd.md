#### 作用

用指定大小的块拷贝一个文件，并进行内容转换和格式处理。使用`man dd`命令获取详细信息。

#### 语法

```bash
dd [operand] ...		# 转换和复制文件
dd <--help|--version>	# 打印帮助信息或版本信息
```

#### 操作数

```
bs=BYTES	# 一次读写BYTES字节，即一个块的大小(默认512)。覆盖ibs和obs
cbs=BYTES	# 一次转换bYTES字节
conv=CONVS	# 用指定的参数转换文件（notrunc:不截短输出文件）
count=N		# 拷贝的块的数量（注：由ibs=指定一个块占多少个字节）
ibs=BYTES	# 一定读取的字节数(默认512)
if=FILE		# 从FILE中输入，而不是stdin
iflag=FLAGS
obs=BYTES
of=FILE		# 输出到FILE，而不是stdout
oflag=FLAG
seek=N		# 从输出文件的N个块后再复制
skip=N		# 从输入文件的N个块后再复制
status=LEVEL	# 输出到stderr的信息。none只显示错误信息，noxfer不显示最终的传输统计信息，progress显示传输统计信息。
```

- N和BYTES可以带后缀：

  c=1, w=2, b=512, kB=1000, K=1024, MB=1000*1000, M=1024\*1024. G, T, P, E, Z, Y也是同样的用法。

- 支持的CONV符号：

  ascii, ebcdic, ibm, block, unblock, lcase, ucase, sparse, swab, sync, excl, nocreat, notrunc, noerror, fdatasync, fsync

- 支持的FLAG符号：

  append, direct, directory, dsync, sync, fullblock, nonblock, noatime, nocache, nofollow, count_bytes, skip_bytes, seek_bytes

#### 选项

```
--help		# 打印帮助信息
--version	# 输出版本信息
```

#### 示例

```
sudo dd if=*.img of=/dev/* bs=4M status=progress	# 创建U盘启动盘
```

