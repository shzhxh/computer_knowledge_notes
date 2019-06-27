#### 简介

Android Debug Bridge，是一个命令行工具，与模拟器或安卓设备交互的桥梁。如果只有一个运行着的模拟器或一个连接着的设备，**adb**会默认与那个设备连接。否则，需要用`-d, -e, -s`选项来指定。

官方文档：http://developer.android.com/tools/help/adb.html

#### 语法

```
adb [选项] 命令
```

#### 全局选项

```
-a	# 
-d	#　使用usb设备
-e	# 使用TCP/IP设备
-s SERIAL	# 使用给定序列号的设备(覆盖$ANDROID_SERIAL)
-t	#
-H	#
-P	#
-L	#
```

#### 命令

##### 连接

```
devices [-l]　　# 显示设备列表

connect host[:port]
disconnect [host[:port]]	# 断开连接

```

##### 文件传输

```
push [--sync] <locals> <remote>	# 将本地文件/目录复制到设备
pull [-a] <remotes> <local>		# 将设备上的文件/目录复制到本地
sync [system|vendor|oem|data|all]	# 将本地build同步到设备，默认为all
```

##### shell

```
shell [options] [commands]	# 运行远程shell命令，如不给出命令则进入交互shell
```

##### 安装软件

```
<install-multiple | install> [-lrtsdg] <packages>
	# 复制packge到设备并安装它，install-multiple用于安装多个package
uninstall [-k] <package>	# 从设备上移除此app package
```

##### 调试

```
logcat [options] [filterspecs]		# 显示设备日志
	# 选项
	-c, --clear	# 清空日志并退出。若定义将日志输出到文件，则清空fileset
	-f, --file=<file>	# 把日志输出到文件。默认是输出到标准输出
	-s	# 将默认过虑器设为silent。等价于filterspec '*:S'
	
	# filterspec的格式
	<tag>[:priority]
	# <tag>是日志里的标签，也可用'*'表示所有的标签
	# priority有如下选项：
	V	详细，<tag>的默认选项
	D	调试，'*'的默认选项
	I	信息
	W	警告
	E	错误
	F	严重错误
	S	安静，即不进行输出
```



##### 安全

```
display-verity
```

##### 脚本处理

```
get-state
get-serialno	# 打印序列号
get-devpath	# 打印设备路径
remount		# 重新挂载/system, /vendor, /oem为可读写
reboot [bootloader|recovery|sideload|sideload-auto-reboot]
	# 重启设备，默认启动系统镜像
	# bootloader表示重启到刷机模式
	# recovery表示重启到恢复模式
root		# 重启adbd开启root权限
unroot		# 重启adbd关闭root权限
usb			# 重启adb服务监听usb
tcpip PORT	# 重启adb服务监听TCP的PORT端口
```

##### 内部调试

```
start-server	# 确保服务在运行
kill-server		# 杀死运行的服务
```



##### 环境变量

```
ADB_TRACE
ANDROID_SERIAL
ANDROID_LOG_TAGS
```


#### 问题汇总
```
问题1： adb server version (40) doesn't match this client (36); killing... 
问题1分析：使用adb version命令，看到Sdk下adb的版本是40，系统里adb的版本是36.我猜是版本冲突，卸载系统里的adb后此问题解决。

问题2：unable to connect to 192.168.0.135:5555: Connection refused
问题2分析：这是由于目标机器上没有开启服务，需要在目标机器的命令行里输入如下命令
  setprop service.adb.tcp.port 5555	// 设置属性，第一个参数是key，第二个参数是value
  stop adbd		
  start adbd	// stop和start两条命令合在一起相当于重启adbd服务
  
问题3: adb connect后，提示offline的问题
问题3分析：我使用的openthos，刚开始没有问题，在openthos休眠再恢复后就提示offline的问题了。adb disconnect后再adb connect，或target和host都重启，问题都未能解决。最终是在target上执行start adbd解决的问题，所以推测是target上adbd出了问题。
```