```shell
trap [-lp] [[arg] <signal_spec>...]	# 捕获信号
	# -l : 打印出信号的名称与它对应的数字
	# -p : 显示与每个SIGNAL_SPEC关联的trap命令
	# <signal_spec> : signal.h里定义的信号名称(SIG前缀可忽略)，或它对应的数字
```

