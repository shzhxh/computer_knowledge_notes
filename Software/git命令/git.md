#### 重要概念

- 分支：每次提交都是一个点，相邻的两次提交连成线，多条线连成一条路径，每条路径就是一个分支。
- HEAD：当前工作区所在的位置，这个位置可以是任意的某个提交。
- 工作区：仓库所在的目录，包括除.git目录外的所有文件和目录。
- 暂存区：使用`git add` 命令把工作区的内容放到暂存区，暂存区在.git目录里。
- 本地仓库：使用`git commit`命令把暂存区的内容放到本地仓库，本地仓库也在.git目录里。
- 远程仓库：使用`git push`命令把本地仓库里的内容放在远程仓库。

#### 语法

```
git [optons] <command> [args]
```

#### 选项

```

```

#### 命令

##### 主命令

```
git-add 	# 把工作区的文件加到暂存区
git-am  	# 打补丁
git-archive # 把文件归档
git-bisect	# 使用二分法查找产生bug的提交
git-branch	# 分支管理
git-bundle	# 通过归档移动对象和引用
git-checkout	# 切换分支
git-cherry-pick	# 对已存在的commit进行再提交
git-citool	#
git-clean	# 删除工作目录中未追踪的文件
git-clone	# 复制仓库
git-commit	# 向仓库中提交
git-describe	# 
git-diff	# 打印各种差别
git-fetch	# 从其它仓库下载对象和引用
git-format-patch
git-gc
git-grep
git-gui
git-init	# 初始化仓库
git-log		# 打印提交的日志
git-merge	# 合并多个开发历史
git-mv		# 移动文件
git-notes	# 
git-pull	# fetch并合并远程仓库(或本地分支)
git-push	# 更新远程仓库
git-rebase
git-reset	# 将HEAD重置到指定的状态
git-revert	# 重置一些已知提交
git-rm		# 从工作树或暂存区中删除文件
git-shortlog
git-show	# 显示对象的各种类型
git-stash
git-status	# 显示工作树的状态
git-submodule	# 子模块管理
git-tag		# 标签管理
git-worktree
gitk
```

##### 辅助命令

```
git-config	# 设置git
git-fast-export
git-fast-import
git-filter-branch
git-mergetool
git-pack-refs
git-prune
git-reflog	# 管理reflog信息
git-remote	# 管理对远程仓库的追踪
git-repack
git-replace
... ...
```

##### 与外界交互

与其它版本管理软件交互或与其他人通过email交互

#### 获取帮助

```shell
man git	# git详解
man gittutorial	# git初学者指引
man gitevery	# git实用命令集
man gitcli	# git命令概述
git help <commond>	# 单个命令commond的信息
```

