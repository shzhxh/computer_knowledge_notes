#### 简介

显示提交之间，或提交与工作目录之间等的差别

#### 语法

```
git diff [options] [--] [<path>...]						# 比较工作区与暂存区之间的差别
git diff --no-index [options] [--] [<path>...]
git diff [options] --cached [<commit>] [--] [<path>...]	# 比较暂存区与commit之间的差别，如果不指定commit则默认为HEAD
git diff [--options] <commit> [--] [<path>...]			# 比较工作区与commit之间的差别
git diff [options] <commit> <commit> [--] [<path>...]	# 比较两个commit之间的差别
git diff [options] <blob> <blob>						# 比较两个blob对象之间的差别
```

#### 选项

```
--stat[=<width>[,<name-width>[,<count>]]]	# 生成diffstat。默认情况下，文件名部分将获得尽可能大的空间，剩余部分才分配给graph部分。
	# <width> : 用于指定宽度。默认为终端的宽度。
	# <name-width> : 用于指定文件名部分的宽度。
	# <count> : 用于限制输出的行数。
```

