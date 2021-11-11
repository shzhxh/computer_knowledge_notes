```
install [options] [-T] <src> <dst>	# 把src复制为dst
install [options] <srcs> <dir>		# 把srcs复制到dir目录
install [options] -t <dir> <srcs>	# 把srcs复制到dir目录
install [options] -d <dirs>			# 为dirs创建所有的组件
```

#### 选项

```
-d, --directory	# 把所有参数都视为目录名，并给这些目录创建所有的组件 
-D	# 创建组件(对于-T选项是除了dst的所有先导组件，对于-t选项是所有组件)，然后把src复制到dst
-m, --mode=<MODE>	# 指定权限模式(就像chmod命令那样，比如0755代表rwxr-xr-x)
-t, --target-directory=<dir>	# 把所有的src都复制进dir目录里 
-T, --no-target-directory		# 把dts视为普通文件
```

