```
sudo [option] <command> # 以其它用户的权限来执行命令
```

#### 选项

```
-H, --set-home	# 将环境变量HOME设置为目标用户的家目录。
-E, --preserve-env	# 保留已有的环境变量，这要求用户要有相应的权限。
```

#### 添加sudo权限

```
# 方法一
visudo	# 或vi /etc/sudoers
	# 模仿root ALL=(ALL) ALL，添加新行user_name ALL=(ALL) ALL
	
# 方法二
usermod -aG sudo <user_name>
	# 在/etc/sudoers可以看到，admin和sudo两个组都有sudo权限，故可把用户加到这些个组以获得sudo权限
	
# 方法三
vi /etc/group	# 直接修改/etc/group命令把用户加到admin或sudo组
```

