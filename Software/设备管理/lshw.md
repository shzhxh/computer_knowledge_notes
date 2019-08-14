```
sudo lshw [options]	# 列出硬件信息。
```

#### 选项

```
-html	# 将设备树输出为HTML格式
-xml	# 将设备树输出为XML格式
-json	# 将设备树输出为JSON格式

-short	# 显示硬件路径
-businfo	# 显示总线信息
-class <class>	# 仅显示给定类型的硬件class的信息。
	# 注：可通过-short或-businfo选项获得可选的class。
-sanitize	# 从输出里删除敏感信息。如IP地址、序列号等。
```

