```
dpkg [options] <action>	# 管理debian包
```

#### 动作

```
-i, --install <packages>	# 安装package
-r, --remove <packages>		# 删除已安装的包package
```

##### dpkg-deb

详见`man 1 dpkg-deb`

```

```

##### dpkg-query

详见`man 1 dpkg-query`

```
-l, --list <packages>	# 列出符合package的包
-s, --status <packages>	# 打印指定包的状态
-L, --listfiles <packages>	# 列出指定包安装在系统中的文件
-S, --search <pattern...>	# 从已安装的包里搜索满足<pattern...>的文件
```



#### 选项

```

```

#### 例子

```

```

#### 错误解决

1 安装缺失的依赖

当`dpkg -i <pkg>`因缺失依赖而失败时，执行`apt install -f`安装缺失的依赖。
