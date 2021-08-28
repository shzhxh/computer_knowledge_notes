#### 简介

管理路由,网络设备,接口和隧道。

#### 语法

```
ip [选项] <实体> <命令 | help>	# 对实体(object)进行管理
ip [-force] -batch <filename>	# 从filename中读取命令
```

#### 实体

```
addrlabel		# 用于IP地址选项的标签配置
l2tp			# 通过IP的以太网隧道
maddress		# 多播地址
macsec			# MACsec设备配置
monitor			# 查找netlink消息
mroute			# 多播路由缓存入口
mrule			# 多播路由策略数据库中的规则
neighbour		# 管理ARP或NDISC缓存入口
netns			# 管理网络命名空间
ntable			# 管理邻居缓存的操作
rule			# 路由策略数据库的规则
tcp_metrics		# 管理tcp metrics
token			# 管理tokenized接口标识
tunnel			# 通过IP的隧道
xfrm			# 管理IPSec策略
```

##### address

协议地址管理。详见`man ip-address`。

```shell
# 可用命令
<add|change|replace>	# 添加、改变、替换协议地址
    <IFADDR>
    <dev IFNAME> 	# 需要添加、改变、替换地址的设备
    [LIFETIME] 
    [CONFFLAG-LIST]
del		# 删除协议地址
    <IFADDR>
    <dev IFNAME>
    [mngtmpaddr]
<save|flush>		# 保存、刷新协议地址
    [dev IFNAME]	# 需要保存、刷新地址的设备
    [scope SCOPE-ID]
    [metric METRIC]
    [to PREFIX]
    [FLAG_LIST]
    [label PATTERN]
    [up]
# 选项
IFADDR : <PREFIX|ADDR peer PREFIX>
    [ broadcast ADDR ] [ anycast ADDR ]
    [ label IFNAME ] [ scope SCOPE-ID ] [ metric METRIC ]
```

##### link

网络设备。详见`man ip-link`。

```shell
# 可用命令
add # 添加仿真链接
    [link DEV] 				# 指定要操作的物理设备
    <[name] NAME>			# 指定新设备的名称
    [txqueuelen PACKETS]
    [address LLADDR]
    [broadcast LLADDR]
    [mtu MTU] 
    [index IDX]
    [numbxqueues QUEUE_COUNT]
    [numrxqueues QUEUE_COUNT]
    <type TYPE> [ARGS]		# 指定新设备的类型
delete [options]
set # 设置设备属性
    <DEVICE | dev DEVICE | group DEVGROUP>
    [up | down]		# 指定设备状态为启动或关闭
    [master DEV]	# 设置当前设备的主设备(即当前设备为从设备)
show [options]
xstats [options]
afstats [dev DEVICE]
help [TYPE]
# TYPE的类型
vlan | veth | vcan | vxcan | dummy | ifb | macvlan | macvtap |
bridge(网桥) | bond | team | ipoib | ip6tnl | ipip | sit | vxlan |
gre | gretap | erspan | ip6gre | ip6gretap | ip6erspan |
vti | nlmon | team_slave | bond_slave | bridge_slave |
ipvlan | ipvtap | geneve | vrf | macsec | netdevsim | rmnet |
xfrm
```

##### route

路由表管理。详见`man ip-route`。

```
# 可用命令
<add|del|change|append|replace>	# 添加、删除、改变、附加、替换路由
    [TYPE] <PREFIX>	# 路由的目的前缀。默认[TYPE]为unicast。<PREFIX>是个地址范围，若为default则代表0/0(IPv4)或::/0(IPv6)。
    [via <FAMILY|ADDRESS>]	# 下一跳路由器的地址。
    [dev DEV]				＃ 输出设备的名字。
```



##### tuntap

管理tun/tap的设备

```
# 可用命令
add, del, show, list, lst, help
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
```

#### 选项

```
-b, --batch <filename>	# 从文件filename中读取命令
--force	# 在batch模式下即使发生错误也不终止命令，只是返回一个非0的错误码。
```

#### 命令

可以使用的命令取决于所操作的实体。常见的命令有`add, del, show, list`。可以使用`ip <实体> help`来获取一个实体的可用命令。
