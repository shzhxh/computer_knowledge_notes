```
lsof [options] [--] [names]		# 列出打开的文件
```

#### 选项

```
-i [i]	# 列出网址满足[i]的文件。
	# [i]的格式如下:
	# [46] [protocol] [@hostname | hostaddr] [:service | port]
	# [46] 用来指定IP协议的版本
	# [protocol] 用来指定协议名(TCP或UDP)
	# [hostname] 主机名
	# [hostaddr] 主机地址
	# [service] 是/etc/services里的名称之一
	# [port] 端口号
```

