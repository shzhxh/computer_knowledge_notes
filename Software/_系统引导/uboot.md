#### 命令

##### 环境变量相关

```
printenv [env]	# 打印环境变量[env]的值，如不指定[env]则打印所有的环境变量
setenv <env> <value>	# 把环境变量<env>的值设置为<value>
saveenv			# 把对环境变量的修改保存到flash里。(否则修改无效)
env default -a	# 恢复所有环境变量的默认值
```

##### 内存操作

```
cp
cmp
load <interface> [<dev[:part]> [<addr> [<filename> [bytes [pos]]]]]	
	# 把文件<filename>从文件系统载入内存。
	# interface - 访问块设备的接口，如mmc, usb, sata等。
	# dev - 设备号。
	# part - 分区号。默认为0，整个设备。
	# addr - 载入地址。
	# filename - 文件名（要包含路径信息）。
	# bytes - 最大载入的字节数。
	# pos - 要跳过的字节数，即开始开始载入的位置。
md[.b, .w, .l, .q] <addr> [num]	# 显示内存里<addr>起[num]个单位的数据
	# md.b 1000 30	# 查看从0x1000起48(0x30)个字节的内存值
nm[.b, .w, .l, .q] <addr>	# 修改内存里<addr>处的值(地址不自增)，输入q退出
mm[.b, .w, .l, .q] <addr>	# 修改内存里<addr>处的值(地址会自增)，输入q退出
mw[.b, .w, .l, .q] <addr> <value> [num]	# 把内存里<addr>起[num]个单位填充为<value>
```

##### 外存操作

```
mmc info	# 当前mmc设备的信息   
mmc read	#
mmc write	#
mmc erase	# 
mmc rescan	#
mmc part	# 列出mmc设备的分区
mmc dev	[dev] [part]
	# 切换mmc设备
mmc list	# 列出有效的mmc设备
mmc hwpartition	#
mmc bootbus	# 
mmc bootpart	# 
mmc partconf	#
mmc rst		#
mmc setdsr	# 
```

##### 文件系统操作

```
fstype	#
ext4ls	#
ext4load	#

```

##### 引导操作

```
boot	# 启动默认配置。即读取环境变量bootcmd来启动Linux。
bootefi [image_addr] [fdt_addr]	#
bootm [addr [args]]	# 启动内存中的Linux.
```

##### 其它

```bash
bdinfo - 板子的信息
version - uboot版本和工具链版本
```



#### 环境变量

bootcmd

bootargs
