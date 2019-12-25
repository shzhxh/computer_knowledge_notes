```
readlink [options] <files>	# 解析符号连接或打印标准文件的名称
```

#### 选项

```
-f, --canonicalize	# 递归地追踪符号链接，要求最终追踪到的那个文件必须存在
-e, --canonicalize-existing	# 递归地追踪符号链接，要求追踪过程中的所有文件必须存在
-m, --canonicalize-missing	# 递归地追踪符号链接，对追踪到的文件是否存在不做要求
-n, --no-newline
-q, --quiet		# 等价于-s
-s, --silent	# 静默模式(默认开启)
-v, --verbose	# 打印错误信息
-z, --zero
--help
--version
```

