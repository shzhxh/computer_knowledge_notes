```
usermod [options] <LOGIN>	# 修改用户账号
```

#### 选项

```
-a, --append	# 把用户加到附加组中且保留原来的主组。只能与-G联用。
-g, --gid <GROUP>	# 设置用户的初始登陆组。用户家目录里的所有文件都将归新组所有。
-G, --groups <GROUP1>[,GROUP2...]	# 把用户加到其它组里。
```

#### 用法

```
usermod -a -G aaa bbb	# 把用户bbb加到组aaa里
```

