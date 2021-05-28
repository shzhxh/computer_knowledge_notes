```
fdisk -l [devices]		# 列出指定设备分区表，如不指定设备则使用/proc/partitions
fdisk [options] device	# 管理磁盘分区表
```

#### 选项

```
-l, --list	# 列出指定设备的分区表
-u, --units[=unit]	# 
```



#### fdisk命令

```bash
# DOS(MBR)
a	# 切换可启动标记
b
c	# 切换DOS兼容性标记

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

# Script
I	# 从脚本载入磁盘布局
O	# 把磁盘布局导出到脚本

# 保存与退出
w	# 将分区表写入磁盘并退出
q	# 退出不保存

# 创建label
g	# 创建GPT分区表
G	# 创建SGI(IRIX)分区表
o	# 创建DOS分区表
s	# 创建Sun分区表
```

