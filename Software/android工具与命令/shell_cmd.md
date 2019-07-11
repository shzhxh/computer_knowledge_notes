

#### dumpsys

```
dumpsys [-t TIMEOUT] [-l | --skip SERVICES | SERVICE [ARGS]]	# 显示所有的服务
	-l		# 只列出服务，而不显示它们的信息
    -t TIMEOUT	# 如不指定TIMEOUT，默认为10秒
    --skip SERVICES	# 除了SERVICES里的服务(用逗号分隔)，显示其它服务的信息
    SERVICE [ARG]	# 只显示服务SERVICE的信息
```

#### 管理器

##### am

```
# 活动管理器
```



##### pm

```
# 包管理器
pm disable <package>	# 使package不可用
pm install				# 安装软件
pm uninstall			# 卸载软件
	-k					# 卸载后保留数据和缓存目录
```

##### wm

```
# 窗口管理器
wm size [reset|WxH]			# 返回或设置显示尺寸
	# 不加参数，显示当前分辨率
	# WxH表示将分辨率设置为宽W高H
	# reset是将分辨率设置为原始分辨率
wm density [reset|DENSITY]	# 设置显示密度
	# 用法与wm size类似
wm overscan [reset|LEFT,TOP,RIGHT,BOTTOM]	# 设置屏幕显示区域
	# 4个参数是距离左，上，右，下的距离
```

#### 系统属性

##### getprop

```
getprop [NAME [DEFAULT]]	# 查看系统属性
```

##### setprop

```
setprop NAME VALUE			# 设置系统属性
```

