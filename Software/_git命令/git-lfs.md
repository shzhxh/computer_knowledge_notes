```
git lfs <command> [args]	# 在git仓库里管理大文件
# 注：此子命令需单独安装：sudo apt install git-lfs
```

高层命令

```shell
install	# 安装配置
pull	# 从远程获取LFS文件。相当于先执行git lfs fetch，再执行git lfs checkout。
	# 选项
	-I, --include=<paths>	# 为当前调用指定lfs.fetchinclude，即要取的文件
	-X, --exclude=<paths>	# 为当前调用指定lfs.fetchexclude，即不要取的文件
	# 注，paths里的各项以逗号分隔
```

底层命令

```
```

