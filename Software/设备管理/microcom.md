```shell
microcom [-d] [-f] [-p devfile | -t host:prot] [-s speed]	# 通过串口访问外设
microcom -c <interface:rx_id:tx_id>	# 以CAN模式访问外设。
microcom -h		# 显示帮助信息。
```

#### 选项

```
-d	# 向stdout输出调试信息
-f	# 忽略lock file
-p, --prot=<devfile>	# 指定串口设备为<devfile>。默认/dev/ttyS0。
-s, --speed=<speed>		# 指定波特率。默认115200。
-t, --telnet=<host:port>	# telnet模式下工作。(rfc2217)
-c, --can=<interface:rx_id:tx_id>	# CAN模式下工作。默认can0:200:200。
-h, --help	# 显示帮助信息
```

