```
mknod [options] <name> <type> [major minor]	# 创建字符或块设备文件
	# <type>的种类
	#	b : 块设备(buffered)
	#	c,u : 字符设备(unbuffered)
	#	p : FIFO
	# [major]的种类 ： 通过`cat /proc/devices`查询
	# [minor]的种类 ： 通过
	# 当<type>是b,c,u的时候，必须设置[major minor]；当<type>是p的时候，必须忽略[major minor]。
```

