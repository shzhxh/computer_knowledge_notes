#### 简介

将当前HEAD重置到指定状态

#### 语法

```
git reset [-q] [<tree-ish>] [--] <paths>...		# 撤销对暂存区的修改,是git add <paths>的反操作
git reset (--patch | -p) [<tree-ish>] [--] [<paths>...]		# git add -p <paths>的反操作
git reset [--soft | --mixed [-N] | --hard | --merge | --keep] [-q] [<commit>]
```



#### 例子

```
git reset HEAD <file>	# 撤销对暂存区的修改
```

