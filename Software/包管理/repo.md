#### 帮助信息

[repo用法详解](https://blog.csdn.net/sunweizhong1024/article/details/8055372)：从源码的角度解释repo的用法

[manifest-format](https://github.com/esrlabs/git-repo/blob/stable/docs/manifest-format.txt)：详细的manifest格式说明

#### 简介

用于管理多个git仓库。其下层调用的是git命令，本身是python脚本。

其设计思想是新建一个仓库来管理要管理的仓库，这个仓库就是manifest仓库，它位于 `.repo`文件夹里。

#### 语法

`repo [command] [options]`

#### 命令

使用`repo help <command>`获取详细信息。

```bash
abandon		# 禁止一个开发分支
branch		# 查看当前分支
branches	# 查看当前分支
checkout	# 检查一个分支
cherry-pick	# cherry-pick一个分支
diff		# 显示commit和工作树之间的区别
diffmanifests	# 显示diff工具
download	# 下载和检查一个分支
forall		# 在每个project运行同样的shell命令
	-r, --regex
	-i, --inverse-regex
	-g GROUPS, --groups=GROUPS
	-c, --command				# 要执行的命令
	-e, --abort-on-errors
gitc-delete	# 删除一个GITC客户端
gitc-init	# 初始化一个GITC客户端
grep		# 打印出若干符合模式的行
help		# 打印出关于命令的帮助信息
info		# 从显示分支，当前分支或未合并分支获取信息
init		# 在当前目录初始化repo
  -u URL	# 指定manifest仓库的地址
  -b REVISION	# 指定manifest仓库的分支或版本
  -m, --manifest-name=<NAME>	# 初始化manifest文件
manifest	# 显示当前使用的manifest信息内容
  -r, --revision-as-HEAD	# 保存当前HEAD的版本
  --suppress-upstream-revision	# 
  -o, --output-file=<file>	# 将输出保存到xml文件
overview	# 显示project未合并分支的概览
prune		# 删除已经合并的分支
rebase		# 使本地分支以上游分支为基础
selfupdate	# 将repo更新到最新的版本
smartsync	# 将工作树更新到最新的完善版本
stage		# 把文件添加到暂存区中
start		# 开始一个新的开发分支
status		# 显示工作树的状态
sync		# 将工作树更新到最新版本
	-c, --current-branch # 只从服务器获取当前分支
	-f, --force-broken 	# 即使某些项目同步失败，也会继续同步其它项目
	--force-sync	# 覆盖已存在的git目录(影响的是.repo/projects目录下的工作树)
	-j JOBS	# 同步下载的JOBS个项目，默认下载4个
upload		# 把代码提交到gerrit code review中
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

##### repo sync的错误
**错误提示**：git prior sync failed; rebase still in progress
解决方法：进到出问题的目录，执行git rebase --abort，提示It looks like git-am is in progress. Cannot rebase.执行git am --abort。再回到原目录执行repo sync即可。
原因分析：由于rebase操作还在进程中，导致sync失败。解决方法就是让rebase退出进程。

**错误提示**：error.GitError: platform/art rev-list ('4c2be345d6bfc25db87f23749912ae9d98d2ad62', 'HEAD', '--'): fatal: bad object HEAD
解决方法：删除art目录，再进行repo sync
原因分析：可能是art目录进行了一些修改，导致git命令执行失败

**错误提示**：access denied or repository not exported
解决方法：清空整个工作区，重新reop sync
原因分析：删除出错的仓库都无效，删除整个工作区才有效，无法理解此问题

#### manifest.xml的格式

所有仓库的信息都放在manifest仓库的xml文件里，其各节点的意义如下：

- **annotation**

- **copyfile**：project的子节点

  src：project的相对路径，

  dest：仓库的相对路径

- **default**：project没有设置时会使用默认配置。

- **extend-project**

- **include**

- **linkfile**：project的子节点，类似copyfile，只是把复制文件变为创建链接文件。

- **manifest**

- **manifest-server**

- **project**：子仓库

  path：子仓库路径，

  name：子仓库名称，

  group：本项目所属的组，

  revison：分支名，

  clone-depth：git克隆的深度

- **remote**：远程仓库地址

  name：名字，

  alias：别名，

  fetch：仓库地址前缀，

  revision：默认分支

- **remove-project**

  删除指定的项目(project)，这样同一manifest文件后续的项目就可以用不同的源(source)来替换这个项目了。这在本地的manifest文件里是很有用的，这样用户就可以用自己定义的项目来替代原有的项目了。

#### 参考文档

- [manifest-format](https://gerrit.googlesource.com/git-repo/+/refs/heads/master/docs/manifest-format.md)



