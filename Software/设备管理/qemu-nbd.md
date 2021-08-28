```
qemu-nbd [options] <filename>	# 使用NBD协议来导出qemu镜像<filename>
qemu-nbd -d <dev>		# 取消到设备dev的连接
```

#### 选项

```
-c, --connect=<dev>	# 连接到设备<dev>
-d, --disconnect	# 取消到设备<dev>的连接
```

