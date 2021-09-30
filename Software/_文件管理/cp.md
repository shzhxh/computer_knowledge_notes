#### 语法

```bash
cp [OPTION] [-T] SOURCE DEST	# 将文件SOURCE复制到文件DEST
cp [OPTION] SOURCE DIRECTORY	# 将若干SOURCE复制到目录DIRECTORY
cp [OPTION] -t DIRECTORY SOURCE	# 将若干SOURCE复制到目录DIRECTORY
```

#### 选项

```
-a, --archive	# 等价于-dR --preserve=all，其意义为递归的复制，但不追踪符号链接，且要保留所有的属性
-d	# 等价于--no-dereference --preserve=links
-f, --force	# 强制覆盖
-P, --no-dereference	# 不追踪符号链接
--preserve[=ATTR_LIST]	# 保留指定属性(默认:mode,ownership,timetamps)，还有附加属性(context,links,xattr,all)
-R: 等价于 -r 或 --recursive，递归地复制
-t DIRECTORY: 等价于 --target-directory=DIRECTORY，将SOURCE复制到文件夹DIRECTORY。
-T 等价于 --no-target-directory， 将DEST作为一般的文件。
```



#### 实例

- 多个文件复制到一个文件夹中

  `cp -t dir a b c d	#将a,b,c,d都复制到文件夹dir中`

- 在当前目录下创建文件夹dir1的副本dir2

  `cp -r dir1 dir2	# dir2不能存在，否则会把dir1复制到dir2目录下`


#### 出错处理

- 复制文件夹时出现`cp: omitting directory`的解决办法

  这是由于文件夹包含子文件夹所致，使用`-r`选项即可

- 把文件复制到fat32分区出现`failed to preserve ownership for`

  fat文件系统不能保存Linux下的权限信息，所以有此提示。其实文件和目录都已正常保存，不用管它。
