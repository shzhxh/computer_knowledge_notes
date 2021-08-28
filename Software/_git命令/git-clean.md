从工作树里移除未追踪的文件

#### 选项

```
-d	# 当没有指定<path>的时候，git clean不会对未追踪的目录进行递归。定义了-d则会对那些目录进行递归。
-f, --force	# 如果git配置变量clean.requireForce设置为false，如不指定-f或-i则git clean不会删除文件或目录。
-i, --interactive	# 交互模式。
-n, --dry-run		# 不进行实际的删除操作，只是显示会发生什么操作。
```

