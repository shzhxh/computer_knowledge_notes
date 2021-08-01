#### 简介

打印网络连接、路由表、接口统计数据、伪装连接和组播成员。

```bash
netstat [-vWeenNcCF] [Af] -r	# 显示内核路由表
# Af : 默认inet
#	-6|-4
#	-A <af>
#	--<af>
#	af可能的值：inet, inet6, ax25, netrom, ipx, ddp, x25
netstat [-vWnNcaeol] [socket...]	# 显示打开的套接字
# socket : 
#	-t, --tcp
#	-u, --udp
#	-U, --udplite
#	-S, --sctp
#	-w, --raw
#	-x, --unix
#	--ax25
#	--ipx
#	--netrom
netstat {[-vWeenNac] -i | [-cnNe] -M | -s [-6tuw]}	# 显示网络接口，或伪装连接，或统计数据。
```



#### 选项

```
# 第一个参数，控制显示的类型
(none)	# 默认显示打开的套接字。
-r	# 显示内核路由表。
-i	# 显示网络接口。
-g	# 显示组播成员。
-s	# 显示每个协议的统计信息。
-M	# 显示伪装连接。

# 其它参数
-v
-W
-n, --numeric	# 显示数字地址，而不是显示符号主机、端口或用户名。
-N
-e
-p, --programs	# 显示PID/程序名
-o
-c， --continuous	# 持续打印信息，时间间隔为1s。

-l, --listening	# 仅列出在监听的服务状态。默认不列出。
-a, --all		# 显示所有的socket。包括在监听的和没有监听的。
-F
-C
-Z
```

#### 示例

```bash
netstat -ntpl	# 显示正在监听中的tcp连接。
	# 第1个参数为none，所以是列出打开的套接字。
	# -t或-tcp，仅列出tcp连接。
	# -n，把主机名和端口号都显示为数字的形式。
	# -p，多出一列来显示pid和程序名。
	# -l，默认显示的是没有在监听中的套接字，使用此选项则恰恰相反，仅显示正在监听中的套接字。
```

