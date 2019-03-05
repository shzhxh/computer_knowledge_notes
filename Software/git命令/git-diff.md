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

