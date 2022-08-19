#### 简介

重写git的修改历史。这是通过添加自定义的过滤器，重写`<rev-list options>`里提到的分支来实现的。

```bash
git filter-branch [options] [--] [<rev-list options>]
```

#### 选项

```bash
--commit-filter <command>
-d <directory>
--env-filter <command>
-f, --force
--index-filter <command>	# 这是一个过滤器，用于重写索引。类似于--tree-filter,但不会check out工作树，这使得它更快。<command>一般是"git rm --cached --ignore-unmatch ..."。
--msg-filter <command>
--original <namespace>
--parent-filter <command>
--prune-empty	# 某些过滤器可能产生空的提交，此选项会删除那些提交。
--setup <command>
--state-branch <branch>
--subdirectory-filter <directory>
--tag-name-filter <command>	# 这是一个过滤器，用于修改tag的名字。
--tree-filter <command>	# 这是一个过滤器，用于重写工作树和它的内容。<command>就是要在shell中执行的命令。
<rev-list options>	# git rev-list的参数。
```

#### 示例

```bash
git filter-branch --tree-filter "rm -f {filepath}" -- --all
	# 从所有提交里删除文件。rm加"-f"的作用是避免某些提交里没有此文件而报错。
```

