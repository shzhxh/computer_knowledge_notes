```
iostat [options] [interval [count]]	# 统计CPU信息，统计磁盘和分区的IO的信息。它属于sysstat包。
	# interval - 时间间隔
	# count - 次数
```

选项

```
-c	# 显示CPU使用情况
-d	# 显示磁盘使用情况
```

示例

```
iostat	# 显示CPU和磁盘的使用情况
iostat -d 2	# 每2秒报告一次磁盘使用情况
iostat -d 2 6	# 每2秒报告一次磁盘使用情况，总共报告6次
```

