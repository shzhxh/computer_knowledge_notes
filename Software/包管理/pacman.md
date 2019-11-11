#### 简介

arch linux 下的包管理工具。在3.0.0之后，pacman就是libalpm(Arch Linux Package Management library)的前端。

#### 语法

```bash
pacman <operation> [options] [targets]
```

####  操作

```
-D,--database	# 操作软件包的数据库。
-Q,--query	 	# 查询软件包数据库。
-R,--remove		# 删除软件包
-S,--sync		# 同步软件包
-T,--deptest	# 检查依赖。**此操作没有选项**。
-U,--upgrade	# 更新或添加软件包
-F,--files		# 查询文件数据库
-V,--version	# 显示版本信息
-h,--help		# 显示帮助信息
```

#### 选项

##### 一般选项

```
-b
-r
-v
--arch
--cachedir
--color
--config
--debug
--gpgdir
--hookdir
--logfile
--noconfirm
--confirm
```

##### 事物选项(-S,-R,-U)

```
-d
--assum-installed
--dbonly
--noprogressbar
--noscriptlet
-p
--print-format
```

##### 升级选项(-S, -U)

```
--force
--asdeps
--asexplicit
--ignore
--ignoregroup
--needed
```

##### 查询选项(-Q)

```
-c
-d
-e
-g
-i,--info	# 查看软件包的信息
-k
-l,--list	# 列出要查询的软件包所包含的文件
-m
-n
-o,--owns <file>	# 查找指定文件所归属的软件包
-q
-s	# 从本地软件包寻找满足正则表达式的结果
-t
-u
```

##### 删除选项(-R)

```
-c
-n
-s	# 递归删除
-u
```

##### 同步选项(-S)

```
-c
-g
-i
-l
-q
-s	# 从同步数据库中搜索满足正则表达式的所有软件包
-u	# 升级所有的软件包
-w
-y	# 从pacman.conf定义的服务器上下载主数据库的新副本
```

##### 数据库选项(-D)

```
--asdeps
--asexplicit
-k
```

##### 文件选项(-F)

```
-y
-l
-s	# 查询包文件名
-x
-o
-q
--machinereadable
```

#### 管理配置文件


#### 使用示例

```bash
pacman -Ss glibc 	# 从软件包数据库中查询glibc
pacman -S python	# 下载并安装python
pacman -U <pkg>		# 本地安装软件包
pacman -Syu			# 更新并升级
pacman -Qs glibc	# 查询满足glibc的本地安装包
pacman -Qo /usr/lib/libm.a	# 查询文件libm.a所归属的软件包
pacman -S --help	# 查询安装软件包的方法
```

#### 错误解决

**错误提示**：error: failed to commit transaction (conflicting files)。

原因分析：这是文件冲突，pacman不会主动覆盖已经存在的文件。

解决方法：将冲突的文件删除，重命名或转移到其它文件夹。

