#### 帮助信息

https://blog.csdn.net/sunweizhong1024/article/details/8055372

#### 简介

获取android源码的工具。是构建于git之上的仓库管理工具，管理了多个git仓库，向版本控制系统进行上传操作，并自动化了部分安卓开发流。repo只是一个python脚本，可以放在任何地方。使用repo，可以把网络上不同位置的仓库下载到本地。

#### 语法

`repo [command] [options]`

#### 命令

使用`repo help <command>`获取详细信息。

```bash
abandon	# 禁止一个开发分支
branch	# 查看当前分支
branches	# 查看当前分支
checkout	# 检查一个分支
cherry-pick	# cherry-pick一个分支
diff	# 显示commit和工作树之间的区别
diffmanifests	# 显示diff工具
download	# 下载和检查一个分支
forall	# 在每个project运行一个shell
gitc-delete	# 删除一个GITC客户端
gitc-init	# 初始化一个GITC客户端
grep	# 打印出若干符合模式的行
help	# 打印出关于命令的帮助信息
info	# 从显示分支，当前分支或未合并分支获取信息
init	# 在当前目录初始化repo
  -u URL	# 指定仓库位置
  -b REVISION	# 指定分支或版本
manifest	# 展示检查工具
overview	# 显示project未合并分支的概览
prune	# 剪掉已经合并的topics
rebase	# 使本地分支以上游分支为基础
selfupdate	# 将repo更新到最新的版本
smartsync	# 将工作树更新到最新的完善版本
stage	# stage files for commit
start	# 开始一个新的开发分支
status	# 显示工作树的状态
sync	# 将工作树更新到最新版本
upload	# upload changes for code review
viersion	# 显示repo的版本
```

#### 使用示例

```
# 查看所有分支
git --git-dir .repo/manifests/.git branch -a

# 复制别人的仓库
# 只需要把.repo目录复制出来即可，不需要全部都复制
# 然后执行repo sync

# .repo目录
ls .repo	# 查看.repo目录
> manifests	# repo配置信息的git库
> manifests.git	# repo配置信息的git库
> manifest.xml	# 到配置信息的软链接
> project.list	# 所有仓库的列表
> project-objects	# 看起来像各个仓库的git配置，只是普通目录不是git仓库
> projects	# 对repo工作目录的备份，只是普通目录不是git仓库
> repo	# repo脚本的git库，包含repo命令的所有实现
```

#### 错误解决

```
现象： 执行repo sync的出现如下错误提示：git prior sync failed; rebase still in progress
解决方法：进到出问题的目录，执行git rebase --abort，提示It looks like git-am is in progress. Cannot rebase.执行git am --abort。再回到原目录执行repo sync即可。
原因分析：由于rebase操作还在进程中，导致sync失败。解决方法就是让rebase退出进程。
```

```
现象：执行repo sync出现如下错误提示：error.GitError: platform/art rev-list ('4c2be345d6bfc25db87f23749912ae9d98d2ad62', 'HEAD', '--'): fatal: bad object HEAD
解决方法：删除art目录，再进行repo sync
原因分析：可能是art目录进行了一些修改，导致git命令执行失败
```

