#### 简介

重写git的修改历史。这是通过添加自定义的过滤器，重写`<rev-list options>`里提到的分支来实现的。

```bash
git filter-branch [options] [--] [<rev-list options>]
```

#### 选项

```bash
--tree-filter <command>	# 这是一个过滤器。用于重写tree和它的内容。<command>就是要在shell中执行的命令。
<rev-list options>	# git rev-list的参数。
```

#### 示例

```bash
git filter-branch --tree-filter "rm -f {filepath}" -- --all
```

