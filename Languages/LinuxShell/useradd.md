#### 简介

创建新用户。

#### 语法

```bash
useradd [options] LOGIN		# 依据options和系统默认值，创建新用户
useradd -D					# 显示当前系统的默认值
useradd -D [options]		# 更新指定选项的默认值，可修改的选项有befgs

# 选项
-b	# 基目录
-c 	# 注释
-d	# 家目录
-D	# 使用系统默认值
-e
-f
-g	# 指定用户所在的组，组名或组id必须已经存在。
-G	# 用户所附属的组
-k	# 框架目录，包含了要向被创建用户主目录中提制的文件和目录。
-K
-l	# 不要将用户添加到lastlog和faillog数据库。默认情况下，将重置lastlog和faillog数据库中的用户条目，以避免重用之前被删除的用户的条目。
-m 	# 创建用户家目录
-M 	# 不创建用户的家目录
-N	# 不创建与用户同名的组
-o	#
-p	# 密码
-r	# 创建系统账户
-R
-s	# 指定用户的shell
-u	# 指定用户id
-U	# 创建与用户同名的组
-z	# SELinux用户
```



#### 实例

```bash
useradd -m -g 用户组 -s /bin/zsh 用户名	# 创建普通用户
```

