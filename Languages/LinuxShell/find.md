#### 语法

`find [options] [starting-point...] [expression]`

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

##### Global options

作用于命令行中所有位置的Tests和Actions操作，总是返回true。

##### Tests

基于文件的属性返回true或false。

```
-empty	# 文件或目录为空
```



##### Actions

执行一些动作，依据动作是否执行成功返回ture或false。

```
-delete		# 删除文件。成功返回ture，失败则返回一个非零的值。使用-delete会自动打开-depth选项，所以应显示地指定-depth以避免意外发生。由于使用-delete就意味着也使用了-depth，所以-delete和-prune是不能一块使用的。
```



##### Operators

将表达式中的其它部分连接起来。

```
! expr		# 对expr的值取反
-not expr	# 等价于"! expr" 
```



#### 不常用文件名

#### 标准一致性

#### 环境变量

#### 例子

```
find ./ -type f -name "hadoop-core*"	# 递归查找当前目录下以hadoop-core开头的文件
find -type f -not -name "*.md"			# 查找类型为文件，名称不匹配"*.md"的文件
```

