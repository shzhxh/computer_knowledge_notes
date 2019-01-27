#### 简介

存储或释放归档文件中的文件。

#### 语法

```
tar {A|c|d|r|t|u|x}[GnSkUWOmpsMBiajJzZhPlRvwo] [ARG...]
```

#### 操作

```
# 第一个参数只能是Acdrtux中的一个。
# A:append
# c:create
# d:differences
# r:append
# t:list
# u:update
# x:extract
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
-f:使用archive file或device ARCHIVE
```

##### 设备屏蔽

##### Archive格式选择

##### 压缩选项

```
-j, --bzip2	# 使用bzip2方式压缩
-z, --gzip, --gunzip, --ungzip	# 使用gzip方式压缩
-Z, --compress, --uncompress	＃ 使用compress方式压缩
```



##### 本地文件选择

```
-C, --directory=DIR	# 执行任何操作前，先切换到DIR目录。

```

信息输出选项

```
-v, --verbose	# 输出详细信息
```



#### 返回值

#### 例子

```
tar zxvf xx.tar.gz
tar jxvf xx.tar.bz2
```



​	