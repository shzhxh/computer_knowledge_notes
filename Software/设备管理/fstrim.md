丢弃已挂载文件系统上的不用的块。

这对于提高SSD的性能很有用。

TRIM是在写入前擦除，而不是写入的同时擦除，以加快驱动器将来的写入。

```
fstrim [options] <mount point>
```

#### 选项

```
-a, --all			# trim所有已挂载的文件系统
-A, --fstab			# trim所有/etc/fstab里已挂载的文件系统
-o
-l
-m
-v, --verbose		# 打印出丢弃的字节数
-n, --dry-run		# 做所有的事，但不trim
```

