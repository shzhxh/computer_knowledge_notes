#### 简介

管理路由,网络设备,接口和隧道

#### 语法

```
ip [选项] <实体> <命令 | help>	# 对实体(object)进行管理
ip [-force] -batch <filename>	# 从filename中读取命令
```

#### 实体

```
address			# 设备的IP地址
addrlabel		# 用于IP地址选项的标签配置
l2tp			# 通过IP的以太网隧道
link			# 网络设备
	# 可用命令
	# add [options]
	# delete [options]
	# set {DEVICE | dev DEVICE | group DEVGROUP}
	#   [up | down]
	# show [options]
	# xstats [options]
	# afstats [dev DEVICE]
	# help [TYPE]
maddress		# 多播地址
macsec			# MACsec设备配置
monitor			# 查找netlink消息
mroute			# 多播路由缓存入口
mrule			# 多播路由策略数据库中的规则
neighbour		# 管理ARP或NDISC缓存入口
netns			# 管理网络命名空间
ntable			# 管理邻居缓存的操作
route			# 路由表入口
rule			# 路由策略数据库的规则
tcp_metrics		# 管理tcp metrics
token			# 管理tokenized接口标识
tunnel			# 通过IP的隧道
tuntap			# 管理tun/tap的设备
	# 可用命令
	# add, del, show, list, lst, help
	# 可用选项
	# dev <PHYS_DEV>
	# mod <tun | tap>
	# user <USER>
	# group <GROUP>
	# one_queue
	# pi
	# vnet_hdr
	# multi_queue
	# name <NAME>
xfrm			# 管理IPSec策略
```

#### 选项

```
-b, --batch <filename>	# 从文件filename中读取命令
--force	# 在batch模式下即使发生错误也不终止命令，只是返回一个非0的错误码。
```

#### 命令

可以使用的命令取决于所操作的实体。常见的命令有`add, del, show, list`。可以使用`ip <实体> help`来获取一个实体的可用命令。
