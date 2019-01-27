#### 语法

```
parted [options] [device [command [options...]...]]
```

#### 选项

```
-h, --help			# 显示帮助信息
-l, --list			# 列出所有块设备的分区情况
-m, --machine		# 显示机器可分析的输出
-s, --script		# 不给用户输出提示信息
-v, --version		# 显示版本号
-a alignment-type, --align alignment-type	# 新分区的对齐方式
```

#### 命令

```
help		# 如果后面跟着命令，就打印该命令的帮助信息，否则打印通用的帮助信息。
align-check	# 检查指定分区的对齐类型。
mklabel		# 创建disklabel
mktable		# 创建分区表
mkpart		# 创建分区
name		# 为分区命名
print		# 打印指定信息
quit		# 退出
rescue		# 恢复丢失分区
resizepart	# 改变分区大小
rm			# 删除分区
select		# 选择要编辑的设备
disk_set	# 改变设备的FLAG
disk_toggle	# 转换设备的FLAG状态
set			# 改变分区的FLAG
unit		# 设备默认unit
toggle		# 转换分区的FLAG状态
version		# 显示版本号和版权信息
```

