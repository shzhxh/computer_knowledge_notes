#### 简介

把改变保存在一个dirty的工作区。它会把工作区和暂存区的状态都保存起来，但只是保存到本地而不会推送到远程。

#### 子命名

```
apply
branch　<branch>	[stash]	# 从stash创建分支
clear	# 删除所有的stash
create
drop	# 移除stash
list	# 列出当前stash
pop	[stash]		# 恢复之前的stash，如不给出stash则默认使用stash@{0}
	# 使用此命令有可能产生冲突，此时需要手动解决冲突，并使用git stash drop命令手动删除对应的stash
push [-m <message>] [--] [pathspec...]	# 把本地的修改保存起来
show	# 显示stash的内容
store
```

