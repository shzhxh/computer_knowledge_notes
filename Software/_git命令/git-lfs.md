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
track	# 查看git lfs路径，或把git lfs路径加到git attributes
```

底层命令

```
```

示例

```bash
# 1. 设置git lfs，每个仓库仅需运行一次
git lfs install
# 2. 选择要追踪的文件，如所有的ISO镜像
git lfs track "*.iso"
# 3. 相关信息存储在".gitattributes"文件里
git add .gitattributes
# 4. 像普通文件那样提交并推送
git add <file>.iso
git commit -m "Add disk image"
git push
```

