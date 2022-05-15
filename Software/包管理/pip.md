#### 简介

python包的管理工具。注，使用`pip command help`可获得比`man pip`更全面的信息。

#### 语法

```
pip <command> [options]		# python2的包管理工具
pip3 <command> [options]	# python3的包管理工具
```

#### 命令

```
help		# 显示帮助
install		# 安装
list		# 列出所有已安装的包
uninstall	# 卸载
freeze		# 按所需格式输出已安装的包
show		# 显示已安装包的信息
search		# 从PyPI查找包
wheel		# 从你的需求构建wheels
```

#### 选项

##### 通用选项

```
-h, --help
--isolated
```



##### install选项

```
-e, --editable <path/url>	# 以可编辑的模式，从本地path或远程url来安装一个项目。
-r, --requirement <file>	# 通过文件<file>进行安装。
-U, --upgrade	# 把所有的包都更新到最新的版本。
--pre			# 默认情况下，pip只寻找稳定版。使用此选项后，将会包含预发布版和开发版。
```



##### uninstall选项