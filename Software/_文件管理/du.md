#### 简介

统计文件的空间占用情况

#### 选项

```
-0, --null
-a, --all
--apparnet-size
-B, --block-size=SIZE
-b, --bytes
-c, --total
-D, --dereference-args
-d, --max-depth=N	# 设置打印的深度，当N为0时等同于--summarize
--files0-from=F
-H	# 等价于-D, --dereference-args
-h, --human-readable	# 以人类易读的形式显示
--inodes	# 不显示块使用情况，而是显示inode使用情况
-k	# 等同于--block-size=1K
-L, --dereference
-l, --count-links
-m	＃ 等价于--block-size=1M
-P, --no-dereference
-S, --separate-dirs
--si	# 类似于-h，但以1000为倍数，而不是以1024为倍数
-s, --summarize	# 只显示每个参数的总计
-t, --threshold=SIZE	# 
--time	# 
--time=WORD
--time-style=STYLE
-X, --exclude-from=FILE
--exclude=PATTERN
-x, --one-file-system
--help	# 打印帮助信息
--version	# 打印版本信息
```

#### 问题解答

1. du和ls显示的文件大小不一致。

   因为du显示的是磁盘占用(disk usage)，而ls显示的是文件大小。磁盘的存储单位是块，所以一般du的结果要比ls大。但也存在du的结果比ls小的情况，这意味着文件实际占用的磁盘空间比它本身要小，原因是文件中有“空”的地方，也就是全0的情况。
