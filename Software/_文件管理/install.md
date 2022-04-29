```
man install	# install命令的用户手册
install [options] [-T] <src> <dst>	# 把src复制为dst
install [options] <srcs> <dir>		# 把srcs复制到dir目录
install [options] -t <dir> <srcs>	# 把srcs复制到dir目录
install [options] -d <dirs>			# 为dirs创建所有的组件
```

#### 选项

```
--backup[=control]	# 为每个已存在的目标文件都创建备份
-b	# 没有参数的--backup
-c	# (已被忽略)
-d, --directory	# 把所有参数都视为目录名，并给这些目录创建所有的组件 
-D	# 创建组件(对于-T选项是除了dst的所有先导组件，对于-t选项是所有组件)，然后把src复制到dst
-m, --mode=<MODE>	# 指定权限模式(就像chmod命令那样，比如0755代表rwxr-xr-x)
-o, --owner=<owner>	# 设置属主(仅超级用户有此权限)
-s, --strip		# 去掉符号表
-S, --suffix=<SUFFIX>	# 备件文件的后缀是"~"，此选项覆盖通常的备份后缀(backup suffix)
	# <SUFFIX>可能的值有：
	#   none, off - 不创建备份(即使使用--backup也无效)
	#   numbered, t - 创建带数字的备份
	#   existing, nil - 如带数字的备份存在则使用带数字的，否则使用普通的
	#   simple, never - 总是创建普通的备份
-t, --target-directory=<dir>	# 把所有的src都复制进dir目录里 
-T, --no-target-directory		# 把dts视为普通文件
```

