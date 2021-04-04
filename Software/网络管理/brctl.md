#### 简介

用于管理网桥。网桥是用于连接不同以太网的设备，每个以太网都会连接到网桥的一个物理接口上。

`brctl`用于取代老旧的`brcfg`工具。

#### 用法

```bash
# 网桥实例的管理
brctl addbr <name>	# 创建名为name的网桥。
brctl delbr <name>	# 删除名为name的网桥。
brctl show			# 显示所有的网桥。

# 网桥端口的管理
brctl addif <brname> <ifname>	# 为网桥brname创建接口ifname.
brctl delif <brname> <ifname>	# 为网桥brname删除接口ifname.
brctl show <brname>		# 显示网桥brname的所有接口。

```

