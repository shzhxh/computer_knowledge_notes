```
truncate <options> <files>	# 修改文件大小
```

#### 选项

```
# 注意：-r和-s选项是互斥的

-c, --no-create		# 不创建文件
-o, --io-blocks		# 将SIZE理解为块的数量。默认是字节的数量。
-r, --reference=RFILE	# 用于文件RFILE的大小
-s, --size=SIZE		# 设置或调整文件大小
	# SIZE可添加后缀单位，如K,M,G等
	# SIZE可添加前缀：+增加，-减小，<至多，>至少，/round down to multiple of，%round up to multiple of
```

