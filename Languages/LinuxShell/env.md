在指定的环境中运行程序。如果什么都不加则列出所有的环境变量。

``` 
env [options] [-] [NAME=VALUE]... [COMMAND [ARGs]]
```

选项

```
-i, --ignore-environment	# 环境为空
-0, --null			# 每行结尾不添加换行符
-u, --unset=NAME	# 从环境中删除变量NAME
-C, --chdir=DIR		# 将工作目录变为DIR
```

