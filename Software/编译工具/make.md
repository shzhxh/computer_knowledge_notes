#### 原理

从根本上说，`make`工具建立的是文件之间相互依赖关系，即一些指定文件的更新触发某种动作。所以，处理文件之间这种依赖关系的问题，都可以用`make`命令。但`make`命令主要还是用在编译大型程序。

要使用`make`，必须要有makefile文件。make是干活的，而makefile告诉make如何干活。

`make`命令的执行过程如下：

1. 读入makefile。
2. 读入被include的其它makefile。
3. 初始化文件中的变量。
4. 推导隐晦规则，并分析所有的规则。
5. 创建文件之间的依赖关系链。
6. 依据依赖关系判断需要重新生成的文件。
7. 执行生成命令。

#### 语法

`make [OPTION] ... [TARGET] ...`

#### OPTIONS

```bash
-b, -m
-B, --always-make
-C dir, --directory=dir	# 更改目录为dir
-d
--debug[=FLAGS]
-e, --environment-overrides
-f file, --file=file, --makefile=FILE	# 使用file作为makefile
-i, --ignore-errors
-I dir, --include-dir=dir
-j [jobs], --jobs[=jobs]	# 指定并行的任务数量为[jobs]。如不写[jobs]，则尽最大可能并行。
--jobserver-fds [R,W]
-k, --keep-going
-l [load], --load-average[=load]
-L, --check-symlink-times
-n, --just-print, --dry-run, --recon	# 只打印命令而不执行它们
-o file
-O [type]
-p, --print-data-base
-q, --question	# 不运行任何命令，也不打印任何东西，只返回一个退出的状态。如果编译目标已经更新则返回0，否则返回其它数字。
-r
-R
-s, --silent, --quiet	# 静默选项。不把正在运行的命令打印出来。
-S,	--no-keep-going, --stop	# 取消-k选项的效果。
-t, --touch	# 更新文件的日期，从而不执行它所依赖的命令。这实际上就是假装命令已经执行过了，以便make将来调用的时候欺骗它。
--trace
-v
-w
--no-print-directory
-W
--warn-undefined-variables
```

#### 示例

```shell
make SHELL="/bin/bash -x"	# 要了解详细的编译过程，但"make -n"又失效的时候，可试试这条命令
```

