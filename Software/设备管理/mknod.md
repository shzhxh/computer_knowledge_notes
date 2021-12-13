```
mknod [options] <name> <type> [major minor]	# 创建字符或块设备文件
	# <type>的种类
	#	b : 块设备(buffered)
	#	c,u : 字符设备(unbuffered)
	#	p : FIFO
	# [major] ： 主设备号，对应了不同设备的驱动程序。内核使用它分派驱动程序。通过`cat /proc/devices`查询。
	# [minor] ： 从设备号，和主设备号一起唯一定位一个设备。在内核里，它会被传给相应的驱动程序。
	# 当<type>是b,c,u的时候，必须设置[major minor]；当<type>是p的时候，必须忽略[major minor]。
	# major或minor：以0x, 0X开头表示是16进制；以0开头表示是8进制；否则，是10进制。
```

选项

```bash
-m, --mode=<MODE>	# 设置文件的权限位MODE
-Z		# 设置SELinux安全上下文为默认类型
--context[=CTX]	# 类似于-Z
--help
--version
```

示例

```bash
```

