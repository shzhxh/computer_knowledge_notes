#### 简介

存储或释放归档文件中的文件。

#### 语法

```
tar {A|c|d|r|t|u|x}[GnSkUWOmpsMBiajJzZhPlRvwo] [ARG...]
```

#### 操作

```
tar {-A|--catenate|--concatenate} [options] <archive> <archive>: 把归档文件添加到其它的归档文件
tar {-c|--create} [-f <achive>] [options] [file...] : 创建归档文件
tar {-d|--diff|--compare} [-f <achive>] [options] [file...]  : 查找归档文件与文件系统的差别
tar --delete [-file <achive>] [options] [member...]: 从归档文件中删除。
tar {-r|--append} [-f <achive>] [options] [file...] : 向归档文件中添加文件
tar {-t|--list} [-f <achive>] [options] [member...] : 列出归档文件的内容
--test-label：测试归档文件的卷标
tar {-u|--update} [-f <achive>] [options] [file...] : 更新归档文件的内容(即用新文件替代归档文件里的内容)
tar {-x|--extract|--get} [-f <achive>] [options] [member...]: 从归档文件中释放文件
--show-defaults ： 打印默认值
-?, --help : 打印帮助信息
--usage ：打印可用的选项
--version : 打印版本信息
```

#### 选项

##### 操作修饰符

```

```

##### 覆盖控制

```

```

##### 输出流选择

```

```

##### 管理文件属性

```

```

##### 扩展文件属性

##### 设备选择与切换

```
-f, --file=<ARCHIVE>	# 使用archive file或device ARCHIVE
```

##### 设备屏蔽

##### Archive格式选择

##### 压缩选项

```
-j, --bzip2	# 使用bzip2方式压缩
-J, --xz	# 使用xz方式压缩
-z, --gzip, --gunzip, --ungzip	# 使用gzip方式压缩，扩展名为.tgz或.tar.gz
-Z, --compress, --uncompress	＃ 使用compress方式压缩
```



##### 本地文件选择

```
-C, --directory=DIR	# 执行任何操作前，先切换到DIR目录。

```

##### 文件名转换

```
--strip-components=<NUM>	# 解压的时候去掉文件名上的<NUM>个前导部分
```



##### 文件名匹配选项

##### 信息输出选项

```
-v, --verbose	# 输出详细信息
```



#### 返回值

#### 例子

```
tar cf abc.tar ./abc	# 给abc目录创建归档文件abc.tar
tar xf abc.tar -C ~/	# 将abc.tar解压到用户目录
tar tf abc.tar			# 列出abc.tar里的文件
```



​	