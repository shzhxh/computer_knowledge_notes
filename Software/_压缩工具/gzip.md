```
gzip [options] [names]	# 压缩或解压
```



#### 选项

```
-[num], --fast, --best	# 用[num]控制压缩速度。-1相当于--fast,代表最快但最弱的压缩。-9相当于--best,代表最慢但最强的压缩。[num]默认为6。
-c, --stdout, --to-stdout	# 输出到标准输出，这样就可以保持原文件不变。
-d, --decompress, --uncompress	# 解压
-f, --force	# 强制压缩或解压缩，不管文件是否有多个链接，不管相关文件是不存在，不管压缩数据是读取自还是写入到终端。
-q, --quiet	# 抑制所有的警告。
-v, --verbose	# 详细模式。
```

