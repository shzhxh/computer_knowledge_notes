```
add-apt-repository [options] <repository>	# 安装或删除一条仓库信息(repository info)
	# 仓库信息可能在/etc/apt/sources.list里，或在/etc/apt/sources.list.d的某个文件里
	# 这条命令只是一个python脚本
	
# <repository>有三种形式
# 1. 可以直接添加到sources.list里的一行。在这种形式下，<repository>是直接添加到/etc/apt/sources.list里的。
# 2. ppa:<user>/<ppa-name>。在这种形式下，将会在/tec/apt/sources.list.d里新建个文件来保存扩展后的完整信息。
# 3. distribution component。在这种形式下，给定的distribution component将会对所有的源都使能。
```

#### 选项

```
-h, --help
-m, --massive-debug
-r, --remove
-y, --yes
-u, --update
-k, --keyserver
-s, --enable-source
```

