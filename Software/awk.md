#### 简介

awk是一种语言，用于处理文本文件。这个名字来源于三个创始人的首字母。

#### 语法

```bash
awk [options] -f <script-file> [--] file	// 用脚本处理文件
awk [options] [--] <script>  file			// 用命令处理文件
```
#### 选项

```bash
-f, --file	# 指定要执行命令的脚本
-F, --field-separator	# 指定域的分割符(即预定义变量FS的值)
```

#### AWK程序执行

```aw
@include "filename"
@load "filename"
pattern {action statement}
function name(parameter list) {statements}
```

#### 变量、记录和域

##### 记录

一般来说，一行就是一条记录。但也可以通过修改变量`RS`来决定划分记录的方式。

##### 域

记录再分割就是域，`FS`是域的划分规则。

\$1, \$2等代表了域的编号，而\$0则代表了记录本身。

变量`NF`代表了一个记录里域的数量。

##### 内建变量

FILENAME : 当前输入文件的名字。

FS : 输入域的分割符，默认为空格。

NF : 当前输入记录里域的数量。

NR : 当前处理的是第几条记录。

OFMT : 数字的输出格式，默认为`%.6g`。

OFS : 输出域的分隔符，默认为空格。

ORS : 输出记录的分隔符，默认为换行符。

RS : 输入记录的分隔符，默认为换行符。

#### 模式和活动

模式在前，活动在后。

##### 模式

```
BEGIN
END
BEGINFILE
ENDFILE
/正则表达式/		# 只有满足正则表达式的输入记录才会被执行
关联表达式
模式 && 模式	# 逻辑与
模式 || 模式	# 逻辑或
模式 ? 模式 : 模式	# 第１个模式为真则使用第２个模式，第１个模式为假则使用第３个模式
(模式)
! 模式		# 逻辑非
模式１，　模式２	# 从满足模式１的记录开始，到满足模式２的记录结束
```



##### 正则表达式

##### 活动

活动是花括号之间的语句`{statements}`。

##### 操作符

##### 控制语句

```
if (condition) statement [else statement]
```



##### I/O语句

```
next		# 停止处理当前的输入记录，读取下一个输入记录继续处理。
print		# 打印当前记录
print expr-list	# 打印表达式expr-list
print expr-list > file	# 将打印出来的表达式重定向到文件
```



##### printf语句

##### 特殊文件名

##### 数字函数

```
cos(expr)	# 余弦，expr是弧度值
rand()		# 返回一个０到１之间的随机数
sin(expr)	# 正弦，expr是弧度值
sqrt(expr)	# 平方根
```



##### 字符串函数

```
length(str)		# 返回字符串的长度
tolower(str)	# 字符转化为小写
toupper(str)	# 字符转化为大写
substr(s,i,[,n])	# 返回从第i的字符开始，最多n个字符的子串。如果不给定n，则返回字符串全部剩余的部分
```



##### 时间函数

##### 位处理函数

##### 国际化函数

#### 用户定义的函数

#### 动态加载新函数

#### 信号

#### 国际化

#### POSIX兼容

#### GNU扩展

#### 环境变量

#### 退出状态

#### 例子

```bash
awk '{print $0}' demo.txt	# 打印demo.txt的内容
```

#### 参考

- [AWK简明教程](https://coolshell.cn/articles/9070.html)
- `man awk`