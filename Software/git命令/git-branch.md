#### 简介

列出、创建或删除分支

#### 语法

```
git branch [options] [patterns]
git branch <--track | --no-track> [-l] [-f] <branchname> [start-point]

git branch <--set-upstream-to=upstream> [branchname]	# 指定branchname追踪远程分支upstream
git branch <--unset-upstream> [branchname]		# 删除对远程分支的追踪

git branch <-m | -M> [oldbranch] <newbranch>	# 移动或重命名分支
git branch <-c | -C> [oldbranch] <newbranch>	# 复制分支
git branch <-d | -D> [-r] <branchname>...		# 删除分支

git branch <--edit-description> [branchname]

# branchname : 要创建或删除的分支名
# start-point: 新分支将要指向的提交点
# oldbranch	: 已存在分支的名称，此分支将被重命名
# newbranch	: 分支的新名称
```



#### 选项

```
-c, --copy		# 复制分支和相关的reflog
-C				# 等价于--copy --force，强制复制分支
-d, --delete	# 删除分支
-D				# 等价于--delete --force，强制删除分支
-f, --force		# 与--track或--no-track联用时，表示强制把branchname重置为start-point
-m, --move		# 移动或重命名分支
-M				# 等价于--move --force，强制重命名分支
-r, --remotes	# 列出远程追踪的分支。如与-d联用，则删除远程追踪的分支。
-t, --track		# 
--no-track		# 
-u <upstream>, --set-upstream-to <upstream>
	# 将upstream指定为branchname的远程分支。如不指定branchname则将upstream指定为当前分支的远程分支。
--unset-upstream	# 删除对远程分支的追踪。如不指定分支则删除当前分支对远程分支的追踪。
-v, -vv, --verbose	# -v和--verbose作用相同，显示每个本地分支的名称、sha1、HEAD的提交信息。-vv则会增加显示本地分支对应的远程分支。
```

