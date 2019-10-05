```
umount -a [options]		# 解除对所有文件系统的挂载
umount [options] <device | directory>	# 解除对指定文件系统的挂载
```

#### 选项

```
-f, --force	# 强制卸载(如果NFS无法访问)
-l, --lazy	# 立即把要卸载的文件系统分离出来，当它不处于busy状态时马上清空对它的引用
```

