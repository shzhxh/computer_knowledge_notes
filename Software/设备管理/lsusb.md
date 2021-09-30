```shell
lsusb [optins]	# 显示系统里usb总线的信息，显示连接在usb总线上的usb设备的信息

# 选项
-v, --verbose			# 显示usb设备的详细信息。
-s [[bus]:][devnum]		# 只显示指定的usb设备。两个id都是10进制的。
-d [vendor]:[product]	# 只显示指定的usb设备。两个id都是16进制的。
-D <device>
-t	# 以树的形式显示usb设备的层次关系
-V, --version	# 显示版本信息
```

