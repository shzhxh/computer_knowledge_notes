#### 简介

包管理系统的命令行接口。它被设计为仅面向普通用户，所以仅包含了最常用的命令和选项。

#### 子命令

```
autoremove	# 曾经因为依赖关系而自动安装的包，现在已不再需要，可用此命令卸载。
autoclean	# 如果一个软件包已被卸载，则删除它的安装文件。
clean		# 删除所有安装文件。
edit-sources	# 编辑sources.list
full-upgrade	#
install		# 
list		#
purge		#  删除包。删除打包数据和配置文件。即使使用了remove命令也可以使用purge命令进行更彻底的删除。但它不会作用于用户目录下的数据和配置。
remove		# 删除包。删除打包数据，但会保留少量的配置文件，这样再次安装就会保留它删除之前的样子。
reinstall	# 重新安装。
satisfy		# 
search		# 
show		# 
update		# 
upgrade		# 
```



#### 用法示例

```
sudo apt install ubuntu-desktop		# install unity
sudo apt install xxxx:i386			# 安装软件包并指定其架构为i386
```

#### 解决错误

##### Failed to fetch

```
# 错误提示：安装 libssl-dev的时候提示：Failed to fetch http://security.ubuntu.com ...
# 解决方法：先运行apt update，再安装
# 原因分析：可能是包对应的网址修改了，所以需要apt update更新数据库
```

##### 通过某ip地址更新

```
# 错误提示：连网失败
# 解决方法：把/etc/apt/apt.conf里的代理注释掉
# 原因分析：设置了代理
```

##### apt-upgrade的问题

```
# 错误提示：dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.
# 解决方法：rm -rf /var/lib/dpkg/updates/*
# 原因分析：配置冲突？
```



#### sources.list文件

- 以`#`开头：注释

- 以deb开头：二进制仓库

  ```
  deb URI section1 section2
  # URI:库所在的地址。dists目录包含了所有软件包的索引，其下的子目录名即为sections1,其中security代表重要的安全更新、updates代表建议的更新、proposed代表预释放的更新、backports代表不支持的更新。打开任意一个子目录，其下的子目录名即为section2，其中main代表完全的自由软件，restricted代表不完全的自由软件，universe代表ubuntu官方不支持，仅靠社区支持的软件，multiverse代表非自由软件，完全不提供支持和补丁。
  ```

  

- 以deb-src开头：源码仓库

