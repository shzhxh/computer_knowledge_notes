列出块设备的信息

```
blkid --label <label>
blkid --uuid <uuid>
blkid [options] [devices]
```

#### 选项

```
-L, --label <label>	# 等价于 --list-one --output device --match-token LABEL=<label>，其作用是查找以label作为文件系统标签的设备。
-o, --output <format>	# 指定输出格式为<format>。<format>可以是：
	full : 打印所有标签(默认)
	value : 打印出标签的值
    list : 以用户友好的格式打印
    device : 只打印设备名
    udev : 以键值对的形式打印，以方便输出到udev环境中
    export : 以键值对的形式打印，以方便输出到环境变量中
-s, --match-tag <tag>	# 只显示满足<tag>标签的设备。
-U, --uuid <uuid>	# 查找以<uuid>作为文件系统uuid的设备。
```

