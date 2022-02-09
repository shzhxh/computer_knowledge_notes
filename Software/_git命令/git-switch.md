```bash
git switch [options] [--no-guess] <branch>
	# 切换到分支<branch>
git switch [options] --detach [start-point]
	# 切换到一个分离的提交点[start-point]
git switch [options] <-c | -C> <new-branch> [start-point]
	# 创建并切换到新分支<new-branch>
git switch [options] --orphan <new-branch>
	# 创建孤儿分支<new-branch>
```

示例

```bash
git switch -	# 切换回之前的分支
```

