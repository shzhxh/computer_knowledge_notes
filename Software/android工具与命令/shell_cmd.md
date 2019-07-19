#### dumpsys

```
dumpsys [-t TIMEOUT] [-l | --skip SERVICES | SERVICE [ARGS]]	# 显示所有的服务
	-l		# 只列出服务，而不显示它们的信息
    -t TIMEOUT	# 如不指定TIMEOUT，默认为10秒
    --skip SERVICES	# 除了SERVICES里的服务(用逗号分隔)，显示其它服务的信息
    SERVICE [ARG]	# 只显示服务SERVICE的信息

# 示例
dumpsys activity [packagename]	# 显示服务activity的信息，即查看正在运行的acitivity
dumpsys battery					# 查看电量信息
dumpsys cpuinfo					# 查看CPU信息
dumpsys meminfo					# 查看内存信息
dumpsys package
dumpsys window | grep mFocused	# 查看处于最上层的窗口信息，可得到包名和活动名
```

#### input

```
input [source] <command> [args]
	# source : dpad, keyboard, mouse, touchpad, gamepad, touchnavigation, joystick, touchscreen, stylus, trackball
	#command
	text <string>							# 向手机输入string
	keyevent <key code number or name>...
	tap <x> <y>
	swipe <x1> <y1> <x2> <y2>
	dragandtrop <x1> <y1> <x2> <y2>
	press
	roll <dx> <dy>
```



#### 管理器

##### am

```
# 活动管理器
am force-stop <package>						# 关闭程序
am start -n <package>/[package].<activity>	# 打开一个活动
am start-activity [option] <INTENT>		# 打开一个活动，它应该和"am start"是等价的
	-D	# 开启调试
	-N	# 开启本地调试
am startservice <service>/.<activity>	# 打开一个服务
am start-service [options] <INTENT>		# 打开一个服务，它应该和"am startservice"是等价的
```



##### pm

```
# 包管理器
pm disable <package>	# 使package不可用
pm clear <package>		# 删除package的所有数据
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

#### 启动或关闭服务

##### start

```
start [services]	# 开启服务，如不指定服务则开启netd/surfaceflinger/zygotes
```

##### stop

```
stop [services]		# 关闭服务,如不指定服务则关闭netd/surfaceflinger/zygotes
```

