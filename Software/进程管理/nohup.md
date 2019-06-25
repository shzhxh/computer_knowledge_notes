```
nohup <command> [args]	# 运行command，忽略hangup信号
	# 当用户注销或远程连接断开时，终端会收到hangup信号，从而关闭其所有子进程
	# 一般会在最后加'&'把命令放后台执行
	# 标准输出和标准错误会默认重定向到nohup.out的文件
```

