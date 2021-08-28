#### 语法

```
git checkout <branch>					# 切换到分支<branch>，如果本地没有此branch而远程有此branch，则等效于git checkout -b <branch> --track <remote>/<branch>
git checkout --detach [<branch>]		# 分离HEAD，切换到branch分支，如果省略branch则仅分离当前分支的HEAD
git checkout [--detach] <commit>		# 分离HEAD并切换到commit
git checkout -b|-B <new_branch> [<start_point>]	# -b创建并切换分支，-B如果不存在则创建，并切换分支
git checkout [<tree-ish>] [--] <pathspec>...	# 通过更新暂存区来覆盖工作目录中的pathspec，如果给定tree-ish，则暂存区和工作树中的pathspec都会被更新。
git checkout (-p|--patch) [<tree-ish>] [--] [<pathspec>...]	# 使用交互界面覆盖pathspec，这样就可以实现更细粒度的控制
```

#### 选项

```
-q, --quiet
--[no-]progress
-f, --force
--ours, --theirs
-b <new_branch>
-B <new_branch>
-t, --track
--no-track
-l

```

#### 用法

```
# 由于github默认只复制远程的master分支，所以如果要复制远程的其它分支应如下操作  
git checkout -b dev remote/dev  #切换到远程dev分支，并对应到本地dev分支

git checkout -- <file>		# 撤销对工作区的修改
```

