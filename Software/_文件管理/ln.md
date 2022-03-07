#### 背景知识：硬链接与软链接的区别
所有硬链接共用一个inode号，所以硬链接本质上是同一个文件。每个软链接都有自己的inode号，所以软链接本质上是不同的文件，只是这个文件本身指向了其它的inode。
* 软链接有私有的文件属性及权限，而硬链接则共享相同的文件属性与权限。
* 可以为不存在的属性或目录创建软链接，而硬链接不行。
* 软链接可以链接到不同的文件系统，而硬链接只能链接到本文件系统中。
* 创建硬链接结点的硬链接计数器会加1,而创建软链接不会。
* 删除软链接不会影响原文件，但所有硬链接被删除后文件本身也会被删除。

#### 语法
```bash
ln [OPTION] [-T] <TARGET> <LINK_NAME>   # 使用名称LINK_NAME创建对TARGET的链接
ln [OPTION] <TARGET>                  # 在当前目录下创建对TARGET的链接
ln [OPTION] <TARGET> <DIRECTORY>     # 在DIRECTORY目录下创建所有TARGET的链接
ln [OPTION] -t <DIRECTORY> <TARGET>  # 在DIRECTORY目录下创建所有TARGET的链接
# 注：默认创建硬链接，使用--symbolic才能创建软链接。
```

#### 参数

```bash
-f, --force		# 如目标文件存在则删除(意为强制创建链接)
-s, --symolic	# 创建软链接
-t, --target-directory=<DIR>	# 将创建的链接放到DIR目录
-T, --no-target-directory	# 将LINK_NAME视为普通文件而非目录
```

#### 示例

```shell
ln -s /home/open/abc abc	# 在前目录下创建软链接abc，其指向/home/open/abc
```

