#### 简介

管理远程仓库

#### 语法

```
git remote <命令> <选项>
```

#### 命令

```
add	<name> <url>	# 添加远程仓库,url是远程仓库的地址,name是远程仓库的名称
rename <old> <new>	# 将远程仓库的名称从old改为new
remove <name>		# 删除对远程仓库name的追踪
set-head <name>		# 为远程仓库设置或删除默认分支
set-branches <name> <branch>	# 改变远程仓库的分支列表
show <name>			# 显示远程仓库name与本地仓库之间分支的对应关系
get-url <name>		# 检索远程分支的url
set-url <name> <newurl>			# 改变远程分支的url
prune [-n] <name>	# 删除与远程分支name相关的陈旧引用 
	-n, --dry-run	# 只显示要删除的分支而不实际删除它们 
update
```

#### 用法

```
# 如果自己从别人那里fork了一个仓库，当别人的仓库更新时，自己的仓库是不会更新的。如果要自己的仓库与别人的仓库同步，应执行如下操作
git remote add upstream https://github.com/original_owner/original_repository.git
git fetch upstream
git checkout master
git merge upstream/master
git push

# 如果clone了一个别人的仓库，但又要关联至自己新建的仓库里以便执行push操作，应执行如下操作
git remote add myrepo https://gitee.com/myname/newrepo.git
git fetch myrepo
git push myrepo local-branch
```

