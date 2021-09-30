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
visudo	# 或vi /etc/sudoers
# 模仿root ALL=(ALL) ALL，添加新行user_name ALL=(ALL) ALL
```

