#### 简介

移动文件

#### 语法

```
mv [option]... [-T] SOURCE DEST			# 将SOURCE重命令为DEST
mv [option]... SOURCE... DIRECTORY		# 将SOURCES移动到DIRECTORY
mv [option]... -t DIRECTORY SOURCE...	# 将SOURCES移动到DIRECTORY
```

#### 选项

```
--backup[=CONTROL]	# 如果目标文件已存在，则生成备份文件
-b		# 类似于--backup，但不接收参数
-f		# 在覆盖前不提示
-i		# 在覆盖前提示
-n		# 不覆盖已有文件
--strip-trailing-slashes
-S		# 
-t		# 将SOURCES移动到DIRECTORY
-T		# 将DEST视为普通文件
-u		# 当SOURCE文件比目标文件新或目标文件缺失，才移动
-v		# 解释下步的动作
-Z		# 
--help
--version
```

#### 注意

1. `mv`命令默认不移动隐藏文件或目录，除非显式地指定它。
