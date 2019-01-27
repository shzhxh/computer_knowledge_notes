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

