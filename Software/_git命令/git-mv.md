```
git mv [options] <src> <dst>	# 重命名文件、目录、符号链接
git mv [options] <srcs> <dst_dir>	# 移动文件、目录、符号链接到<dst_dir>
	# 注：git-mv也可以移动子模块。
	-f, --force	# 即使目标已存在，也强制重命名或移动
	-k	# 如移动或重命名会引起错误，则跳过
	-n, --dry-run	# 只显示动作，不实际执行
	-v, --verbose	# 详细模式
```

