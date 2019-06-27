#### 简介

显示或设置网络设备。

它一般是在启动的时候用来设置网络设备，启动后只有在调试和系统调整的时候才用到它。



#### 语法

```
ifconfig [-v] [-a] [-s] [interface]		# 显示接口状态。如不带参数，则显示当前活动的接口的状态。
	# -a	显示所有接口的状态，包括不活动的接口
	# -s	显示简短信息，相当于netstat -i
	# -v	对于一些错误情况显示更多信息
	# interface	接口名，如给出此参数则显示该接口的状态。
ifconfig [-v] interface [aftype] options | address ...	# 配置接口
	# aftype是指地址族的类型(Address Families)
	# address是指要赋予接口的IP地址
```

#### 地址族

```
inet	# TCP/IP，默认
inet6	# IPv6
ax25	# AMPR Packet Radio
ddp		# Appletalk Phase 2
ipx		# Novell IPX
netrom	# AMPR Packet radio
```

#### 选项

```
up		# 使接口活动
down	# 使接口关闭
[-]arp	# 关闭或启动ARP协议
[-]promisc		# 关闭或启动promiscuous模式
[-]allmulti
mtu N			# 设置最大传输单元为N
dstaddr addr
netmask addr			# 设置子网掩码
add addr/prefixlen		# 设置IPv6的地址
del addr/prefixlen		# 删除IPv6的地址
tunnel ::aa.bb.cc.dd	# 建立IPv4与IPv6的隧道通信
irq addr				# 设置网络设备的IRQ
io_addr addr			# 设置网络设备的I/O地址
mem_start addr			# 设置网络设备在内存中的起始地址
media type				# 设置网络设备的媒介类型
[-]broadcast [addr]		# 向addr广播数据包
[-]pointopoint [addr]	# 与addr建立点对点连接，此模式具有保密功能
hw class address		# 设置网络设备的类型与硬件地址
multicast
txqueuelen length
```

