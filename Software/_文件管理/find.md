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

##### Positional options

作用于命令行中它后面的Tests和Actions操作，总是返回true。

```
-daystart
-follow
-regextype <type>
-warn
-nowarn
```

##### Global options

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



##### Tests

基于文件的属性返回true或false。

```
-empty	# 文件或目录为空
-name <pattern>	# 查找满足<pattern>的文件
-path <pattern>	# 查找满足<pattern>的目录
-size <n>	# 指定文件大小。可以带后缀表示单位，可以带前缀表示范围。
	# 可用的后缀：b,512字节的块；c，字节；w，双字节；k，1024字节；M,1024k；G,1024M。
	# 可用的前缀：+,大于；-,小于。
```



##### Actions

执行一些动作，依据动作是否执行成功返回true或false。

```
-delete		# 删除文件。成功返回ture，失败则返回一个非零的值。由于使用-delete就意味着也使用了-depth，所以-delete和-prune是不能一块使用的。注意：此选项应放在最后，因为它是根据它前面的选项做删除的。
-exec <cmd>	# 
-print	# 打印完整文件名，后跟\n字符。
-print0	# 打印完整文件名，后跟NULL字符。
-printf <fmt>
-prune	# 忽略匹配的目录。但如使用了-depth选项，则此选项无效。
-quit	# 
```



##### Operators

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
find ./ -type f -name "hadoop-core*"	# 递归查找当前目录下以hadoop-core开头的文件
find -type f -not -name "*.md"			# 查找类型为文件，名称不匹配"*.md"的文件
find . -type f -size +1M -print0 | xargs -0 du -h	# 查找当前目录下大于1M的文件，并打印其大小。注：如果想排序的话还可以通过管道输出给sort -h，但不要使用sort -n，因为-n选项是按数字排序，并不能表达文件真实的大小序列
```

