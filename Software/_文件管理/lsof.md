```
lsof [options] [--] [names]		# 列出进程打开的文件，或打开文件的进程，或进程打开的端口。
```

#### 选项

```
-a		# 执行布尔AND运算
-d <FDs>	# 指定文件描述符的列表以选择输出或不输出它们。文件描述符之间以逗号分隔且不能出现空格。
-i [i]	# 列出网址满足[i]的文件。
	# [i]的格式如下:
	# [46] [protocol] [@hostname | hostaddr] [:service | port]
	# [46] 用来指定IP协议的版本
	# [protocol] 用来指定协议名(TCP或UDP)
	# [hostname] 主机名
	# [hostaddr] 主机地址
	# [service] 是/etc/services里的名称之一
	# [port] 端口号
-p [PIDs]	# 指定PID的列表以选择输出或不输出它们的文件。
```

#### 示例

```
lsof /etc/passwd	# 查看哪个进程在占用/etc/passwd
```

