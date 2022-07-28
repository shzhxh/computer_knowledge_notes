#### 语法

````
find [options] [starting-point...] [expression]
	# 查找文件
	# start-point : 查找的起点。如不指定则为当前目录。
````

#### 选项

```
-H	# 不追踪符号链接，但在命令行里指定的符号链接除外。
-P	# 不追踪符号链接，即只打印符号链接本身的属性。这是默认行为。
-L	# 追踪符号链接，将打印符号链接指向的那个真实文件的属性。
-D debugoptions	# 打印诊断信息，这样有助于查询find命令为什么没有按预想中那样执行。使用find -D help获取详情。
-Olevel		# 优化查询，共有从0到3共计4个优化级别。
```

#### 表达式

表达式是一种查询规范，描述了如何匹配文件。表达式由如下部分组成。

##### 局部选项

作用于命令行中它后面的Tests和Actions操作，总是返回true。

```
-daystart
-follow
-regextype <type>
-warn
-nowarn
```

##### 全局选项

作用于命令行中所有位置的Tests和Actions操作，总是返回true。

```bash
-d	# 等价于-depth，为了适应各种BSD Unix和MacOs X。
-depth	# 
-help
-ignore_readdir_race
-maxdepth <levels>	# 指定要查找的文件夹的层数
-mindepth <levels>
-mount
-noignore_readdir_race
-noleaf
-version
-xdev
```



##### 测试

基于文件的属性返回true或false。

```
-amin <n>	# 文件的最后访问时间是n分钟之前。n前面可以加+/-号表示高于/低于。
-atime <n>	# 文件的最后访问时间是n天之前。n前面可以加+/-号表示高于/低于。
-cmin <n>	# 文件状态的最后修改时间是n分钟之前。n前面可以加+/-号表示高于/低于。
-ctime <n>	# 文件状态的最后修改时间是n天之前。n前面可以加+/-号表示高于/低于。
-empty	# 文件或目录为空
-iname <pattern>	# 查找满足<pattern>的文件，但大小写不敏感。
-mmin <n>	# 文件的最后修改时间是n分钟之前。n前面可以加+/-号表示高于/低于。
-mtime <n>	# 文件的最后修改时间是n天之前。n前面可以加+/-号表示高于/低于。
-name <pattern>	# 查找满足<pattern>的文件
-path <pattern>	# 查找满足<pattern>的目录
-size <n>	# 指定文件大小。可以带后缀表示单位，可以带前缀表示范围。
	# 可用的后缀：b,512字节的块；c，字节；w，双字节；k，1024字节；M,1024k；G,1024M。
	# 可用的前缀：+,大于；-,小于。
-type <c>	# 文件类型为<c>，可用用逗号分隔的字符来表示多种类型
	# 类型的值：b，块设备；c，字符设备；d，目录；p，有名管道；f，普通文件；l，符号链接；s，网络插座；D，门(Solaris操作系统)
```



##### 动作

执行一些动作，依据动作是否执行成功返回true或false。

```
-delete		# 删除文件。成功返回ture，失败则返回一个非零的值。由于使用-delete就意味着也使用了-depth，所以-delete和-prune是不能一块使用的。注意：此选项应放在最后，因为它是根据它前面的选项做删除的。
-exec <cmd> \;	# 执行命令cmd，在命令的最后必须以\;结束 
-exec <cmd> {} +	#
-execdir <cmd>	#
-execdir <cmd> {} +	#
-fls <file>	#
-fprint <file>	#
-fprint0 <file>	#
-fprintf <file> <fmt>	#
-ls
ok <cmd>
-okdir <cmd>
-print	# 打印完整文件名，后跟\n字符。
-print0	# 打印完整文件名，后跟NULL字符。
-printf <fmt>
-prune	# 忽略匹配的目录。但如使用了-depth选项，则此选项无效。
-quit	# 
```



##### 操作符

将表达式中的其它部分连接起来。如下操作符的优先级是递减的：

```
(expr)		# 强制使expr有最高优先级
! expr		# 非运算。对expr的值取反
-not expr	# 等价于"! expr"，但不是POSIX标准。 
expr1 expr2	# 且运算。
expr1 -a expr2		# 等价于expr1 expr2。
expr1 -and expr2	# 等价于expr1 expr2，但不是POSIX标准。
expr1 -o expr2	# 或运算。
expr1 -or expr2	# 等价于expr1 -o expr2，但不是POSIX标准。
expr1 , expr2	# expr1和expr2的值都会计算，但只取expr2的值。
```



#### 异常文件名

文件名中除了`\0`和`/`之外，可以包含任意字符。那么包含了异常字符的文件名可能会产生潜在的问题，比如改变一些终端里功能键的设置。find命令里actions表达式对异常字符的处理是多样的。

#### 标准的一致性

为了最适合POSIX标准，应设置环境变量`POSIXLY_CORRECT`。

#### 环境变量

#### 例子

```
# 通过文件名进行查找
find ./ -type f -name "hadoop-core*"	# 递归查找当前目录下以hadoop-core开头的文件
find -type f -not -name "*.md"			# 查找类型为文件，名称不匹配"*.md"的文件
find -type f ! -name *so -a ! -name *exe	# 查找即不以so结尾，也不以exe结尾的文件

# 通过目录名进行查找
find -path ./abc -prune -o -name "*rs"	# 查找以rs结尾的文件，但不在abc目录下查找

# 通过文件大小进行查找
find . -type f -size +1M -print0 | xargs -0 du -h	# 查找当前目录下大于1M的文件，并打印其大小。注：如果想排序的话还可以通过管道输出给sort -h，但不要使用sort -n，因为-n选项是按数字排序，并不能表达文件真实的大小序列
find -size +100k -size -500k	# 查找介于100k到500k之间的文件。注：查找范围必须使用相同的单位，否则会查找失败

# 查找完成后进行一些操作
find -type f -name "*.png" -exec cp {} /tmp/png/ \;	# 把当前目录下的png文件复制到/tmp/png/目录下
find abc -type d -empty -delete	# 递归删除abc下所有的空目录（包括abc）

```

