#### 简介

将当前HEAD重置到指定状态

#### 语法

```
git reset [-q] [tree-ish] [--] <paths>...		# 撤销对暂存区的修改,是git add <paths>的反操作
git reset (--patch | -p) [<tree-ish>] [--] [<paths>...]		# git add -p <paths>的反操作
git reset <mode> [<commit>]	# 将当前分支的head指向commit
	# mode是如下选项之一：
	--soft : 不动暂存区和工作区，只是把head指向commit。
	--mixed: 重置暂存区但保留工作区，并报告哪些没有更新。这是默认操作。
	--hard : 重置工作区和暂存区。这意味着commit之后所有对工作区的修改将被丢弃。
	--merge:
	--keep :
```



#### 例子

```
git reset HEAD <file>	# 撤销对暂存区的修改
git reset HEAD^			# 撤销最近一次提交
git reset HEAD^^^		# 撤销最近三次提交
git reset HEAD~3		# 撤销最近三次提交
```

