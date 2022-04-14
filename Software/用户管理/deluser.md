#### 简介

从系统中删除用户或组。由于是面向于用户前端，所以相比于userdel和groupdel，deluser和delgroup更友好一些。deluser只删除用户，默认不删除home directory, mail spool 或其它属于用户的文件。

#### 语法

```
deluser [options] user			# 删除普通用户
deluser --group [options] group	# 删除组
delgroup [options] group		# 删除组
deluser [options] user group	# 从指定的组里删除用户
```

#### 选项

```
--remove-home：删除home directory和mail spool
--remove-all-files:删除所有属于用户的文件
--system	# 仅删除系统用户或系统组。
```

