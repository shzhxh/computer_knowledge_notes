#### 简介

设置属性

#### 语法

#### 选项

```
--global	# 修改~/.gitconfig里的配置文件，但如果$XDG_CONFIG_HOME/git/config存在则修改之，而不再修改~/.gitconfig。
--system	# 修改/etc/gitconfig
--local		# 修改.git/config里的配置文件
```



#### 配置

#### 文件

#### 环境

#### 示例

#### 配置文件

##### 语法

##### 包含

##### 条件包含

##### 示例

##### 值

##### 变量

```
core.quotePath	# 把路径里的不常用字符用反斜杠的方式来编码，当这个变量设置为false的时候，字符均不认为是不常用的。
```

#### 使用示例

```
# 执行git status命令不能正常显示中文
git config --global core.quotepath false
```

