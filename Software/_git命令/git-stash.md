#### 简介

把改变保存在一个dirty的工作区。它会把工作区和暂存区的状态都保存起来，但只是保存到本地而不会推送到远程。

```
git stash <subcommand> [options]
```



#### 子命名

```
branch　<branch>	[stash]	# 从stash创建分支
create
show	# 显示stash的内容
```

##### apply

```bash
git stash apply [options] [stash]	# 把一个stash添加到工作区，但不会从stash列表里删除对应stash

# 选项
--index
-q, --quiet
```

##### clear

```bash
clear	# 删除所有的stash

```



##### drop

```bash
git stash drop [-q| --quiet] [stash]	# 移除stash

```



##### list

```bash
git stash list [options]	# 列出当前stash

```

##### pop

```bash
git stash pop [options]	[stash]		# 从stash列表里删除一个stash，并把它添加到工作区，如不给出stash则默认使用stash@{0}
	# 使用此命令有可能产生冲突，此时需要手动解决冲突，并使用git stash drop命令手动删除对应的stash
	
# 选项
--index
-q, --quiet
```



##### push

```bash
git stash [push] [options] [--] [pathspec...]	# 把本地的修改保存起来，并回滚到HEAD

# 选项
-p, --patch
-k, --[no-]keep-index
-q, --quiet
-u, --include-untracked
-a, --all
-m, --message <msg>

```

##### save

已弃用，被`push`子命令取代。

##### store

```bash
git stash store
```

