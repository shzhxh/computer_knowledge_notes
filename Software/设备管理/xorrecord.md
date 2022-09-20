```
xorrecord [options] dev=<device> [track_source]	# 向CD, DVD, BD写入格式化的数据。
```

选项

```
# 定位设备
	dev=<device>	# 设置要使用设备的libburn地址。比如dev=/dev/sr0。
# 查询设备
# 烧写相关的设置
	blank=<mode>	# 让CD-RW或DVD-RW变为空的，这样就能重新使用它了。
	-eject			# 工作完成后弹出驱动器托盘。
	fs=<size>		# 设置fifo缓冲区的大小。默认为4m。
	padsize=size	# 
	speed=<value>	# 设置写的速度。
# 其它设置
	-v	# 详细模式。
# 与cdrecord不兼容的设置
```

示例

```
# 浏览设备
xorrecord --devices
# 写入单个iso镜像
xorrecord -v dev=/dev/sr0 speed=12 fs=8m blank=as_needed -eject padsize=300k my_image.iso
```

