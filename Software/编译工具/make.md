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

```
-b, -m
-B, --always-make
-C dir, --directory=dir	# 更改目录为dir
-d
--debug[=FLAGS]
-e, --environment-overrides
-f file, --file=file, --makefile=FILE	# 使用file作为makefile
-i, --ignore-errors
-I dir, --include-dir=dir
-j [jobs], --jobs[=jobs]
--jobserver-fds [R,W]
-k, --keep-going
-l [load], --load-average[=load]
-L, --check-symlink-times
-n, --just-print, --dry-run, --recon	# 只打印命令而不执行它们
-o file
-O [type]
-p, --print-data-base
-q
-r
-R
-s
-S
-t
--trace
-v
-w
--no-print-directory
-W
--warn-undefined-variables
```