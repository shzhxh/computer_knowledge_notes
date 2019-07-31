```
git merge [options] [-m msg] [commit...]	# 将commits的改变合并到当前分支
	# commits一般是要合并到当前分支的其它分支的heads，如不指定commits则试图将远程分支合并到当前分支
git merge --abort		# 当合并出现冲突后，终止合并的进程并尝试回到合并前的状态
git merge --continue	# 当合并出现冲突并解决完冲突后，可以用此命令继续完成合并
```

