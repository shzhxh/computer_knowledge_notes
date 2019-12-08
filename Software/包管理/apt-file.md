#### 语法

```
apt-file [options] <action> [pattern]
apt-file [options] -f <action> <file>
apt-file [options] -D <action> <debfile>
```

#### 动作

```
update			# 从apt-sources获取内容
search, find	# 查找
list, show		# 列出包里的文件
```

#### 选项

##### 模式(pattern)选项

```
-f, --from-file	# 从文件读取模式，每行代表一个模式(使用"-"代表从stdin读取模式)
-D, --from-deb	# 使用.deb包的文件列表作为模式
```



##### 查找选项

##### 其它选项

#### 使用

- 安装完后要更新数据库`apt-file update`