#### 简介

配置蓝牙连接，或向蓝牙设备发送一些特殊命令。

#### 语法

```
hcitool [-h]		# 打印帮助信息
hcitool [-i <hciX>] <command> [command parameters]	# 向蓝牙设备发送命令command。如果没有使用-i选项指定蓝牙设备hciX，则默认使用第一个被发现的蓝牙设备。
```

#### 命令

```
dev		# 列出本地设备
inq
scan	# 查询远程设备。会列出设备地址与名称。
name <bdaddr>
info <bdaddr>
spinq
epinq
cmd
cc
dc
sr
cpt
rssi
lq
tpl
afh
lp
lst
auth
enc
key
clkoff		# 读取蓝牙设备的时钟偏移量
clock		# 读取蓝牙设备的时钟
lescan		# LE扫描
leinfo		# LE远程信息
lerladd		# LE解析列表里添加设备
lerlrm		# LE解析列表里删除设备
lerlclr		# LE解析列表清空
lerlsz		# LE解析列表的大小
lerlon		# LE地址解析开启
lerloff		# LE地址解析关闭
lecc		# LE连接创建
ledc		# LE连接断开
lecup		# LE连接更新
```

