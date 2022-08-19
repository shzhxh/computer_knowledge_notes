#### 简介

个人认为，git-lfs存在着比较大的问题，尽量不要使用。git-lfs的本质是用其它的服务器来保存大文件，这就打破了仓库的完整性且无法恢复，会带来很多的问题。

如果仓库里存在着大文件，第一应考虑的是这些大文件是必须的吗？如果是必须的，第二应考虑的是怎么生成、保存、获取它们？而不是使用git-lfs。

```
git lfs <command> [args]	# 在git仓库里管理大文件
# 注：此子命令需单独安装：sudo apt install git-lfs
```

#### 高层命令

```shell
env
checkout
dedup
ext
fetch
fsck
install	# 安装LFS的配置
lock
logs
ls-files	# 显示git lfs文件的信息，包括索引里的和工作目录里的。
migrate	# 迁移的历史(to or from git-lfs)
	info	# 显示仓库尺寸的信息。
prune
pull	# 从远程获取LFS文件。相当于先执行git lfs fetch，再执行git lfs checkout。
	# 选项
	-I, --include=<paths>	# 为当前调用指定lfs.fetchinclude，即要取的文件
	-X, --exclude=<paths>	# 为当前调用指定lfs.fetchexclude，即不要取的文件
	# 注，paths里的各项以逗号分隔
push
status
track	# 查看git lfs路径，或把git lfs路径加到git attributes
uninstall	# 卸载git lfs,包括删除hooks,清空配置。
unlock
untrack		# 从git attributes里删除git lfs路径。
update
version
```

#### 底层命令

```
clean
filter-process
pointer
post-checkout
post-commit
post-merge
pre-push
smudge
standalone-file
```

#### 示例

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
# 5. 卸载git lfs
git lfs uninstall
git rm --cached <lfs-file>	# 删除所有.gitattributes里追踪的文件
rm .gitattributes
git add .gitattributes
git commit -m "uninstall git-lfs"
# 6. 从提交历史里彻底去掉git-lfs
	# 因为github服务器上始终保保留了lfs存在的痕迹，所以唯一的方案可能就是删除再重建了
	# 这个方法的缺点是：与仓库相关的所有东西都无法保留，比如fork关系、issure、pr记录等
	# 第一步，github上先删除，再新建仓库
	# 第二步，卸载git lfs
	# 第三步，使用git filter-branch命令把lfs相关文件从历史中删除。
	rm -r .git/lfs	# 删除lfs
	rm `grep -ril lfs .git/hooks`	# 删除相关的勾子
	git push	# 在保留了提交记录的前提下，github服务器上不再有lfs存在的痕迹了
```

#### 错误解决

##### 1 

错误提示：执行`git lfs pull`产生这样的错误`Smudge error: Error downloading ... Not Found: [404] Not Found`

原因分析：远程仓库的大文件不存在

解决方法：先要在本地把这些大文件下载完整，然后使用`git pull` 命令推送到远程仓库。
