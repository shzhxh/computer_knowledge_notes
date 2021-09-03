```
jiri [flags] <cmd>	# 多仓库管理工具
jiri help <cmd>		# <cmd>的用法
jiri help <topic>	# 关于某个主题的信息
	# 可选topic有：filesystem, manifest-files
```

import子命令

```
jiri import [flags] <manifest> <remote>	
	# 向.jiri_manifest文件里添加import。文件.jiri_manifest里定义了清单信息。
	# <manifest>定义了本地清单文件。
	# <remote>定义了远程清单仓库。
	# 独属于import的标志：
	-name=<manifest>	# 远程清单仓库的名称
```

init子命令

```
init [flags] [dir]
	# 创建jiri根。其实就是创建.jiri_root目录和及模板文件。
	# [dir]指出了要init命令运行的目录。
	# 独属于init的标志：
	-keep-git-hooks=<true|false>	# 执行jiri update的时候是否保持.git/hooks下的git钩子
```

update子命令

```
update [flags] <file|url>
	# 更新所有的jiri项目。这些要更新的项目都定义在清单文件里。
	# 独属于update的标志：
	-run-hooks=<true|false>	# 在更新源代码后是否运行钩子。
```



全部子命令都可使用的标志

```
-v	# 打印出debug级的信息
-vv	# 打印出trace级的信息
```

