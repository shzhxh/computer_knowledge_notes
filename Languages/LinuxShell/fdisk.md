管理磁盘分区表

```
fdisk -l [devices]		# 列出指定设备分区表，如不指定设备则使用/proc/partitions
fdisk [options] device
```

#### fdisk命令

```
# DOS(MBR)
a	# 切换可启动标记
b
c

# Generic
d	# 删除分区
F
l	# 显示分区类型
n	# 创建新分区
p	# 打印分区表
t	# 改变分区类型
v	# 验证分区表
i

# Misc
m	# 显示帮助信息
u
x	# 扩展功能(experts only)

Script
I
O

# 保存与退出
w	# 将分区表写入磁盘并退出
q	# 退出不保存

# 创建标签
g
G
o
s
```

