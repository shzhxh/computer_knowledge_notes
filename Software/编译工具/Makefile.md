#### 总述

	Makefile = 显示规则 + 隐晦规则 + 变量定义 + 文件指示 + 注释
​	引用文件：

```makefile
include <filename>	# 把别的文件包含进来，但文件不存在会报错
-include <filename>	# 把别的文件包含进来，即使文件不存在也不报错
```

​	环境变量：MAKEFILES（建议不要使用）

#### 书写规则

	规则 = 依赖关系 + 生成目标的方法
​	语法：
​		targets: prerequisites
​		[TAB]command
​	文件路径： VPATH vpath

#### 书写命令

#### 使用变量

- 变量使用的时候前面要加\$符号，字符'\$'要用\$$来表示。
- `=`：直接赋值。右边的内容会按原样存储。如果右边是变量引用，则存储的是引用本身，而不是变量的值，左边的变量只有实际被使用时右边的变量引用才会真正地展开。
- `:=`：简单赋值。类似于传统程序语言的赋值。对右边的表达式求值，将结果存储在变量中。
- `?=` ：条件变量赋值，被赋值的变量之前没有被赋值过才会被赋值。
- `+=`：给变量追加赋值。
- override用于设置命令行里由make指定的参数
- define关键字用于设置多行变量，末尾以endef结束。
- '\$@'，'\$<'，'\$^'详见 [自动化变量](#自动化变量)
- `$(var: a=b)`：获取变量var的值，用b代替词尾的a

#### 条件判断

`[ifeq | ifneq | ifdef | ifndef] ... else ... endif`

#### 使用函数

函数调用的语法如下：

```
$(<function> <arguments>)	# 调用函数function，其中arguments为其参数
```



##### 字符串处理函数
```
$(filter <pattern...>,<text>)	
	# 从字符串<text>中过滤出符合<pattern>模式的单词
$(filter-out)
$(findstring)
$(firstword)
$(patsubst PATTERN,REPLACEMENT,TEXT)
	# 将TEXT中符合PATTERN的单词替换为PEPLACEMENT
$(sort <list>)
	# 给字符串<list>中的单词按升序排序，返回排序后的字符串。
$(strip)
$(subst <from>,<to>,<text>)	# 把字符串<text>中的<from>替换成<to>。
$(wildcard <PATTERN...>)	 
	# 在变量定义和函数引用时，通配符会失效。此时要让通配符有效，就要使用wildcard。
$(word <n>,<text>)	# 取字符串<text>中第<n>个单词
$(wordlist <ss>,<e>,<text>)		
	# <ss>和<e>是单个数字，列出<text>里从序号<ss>到序号<e>之间的单词
$(words <text>)		# 统计<text>里的单词个数
```
##### 文件名操作函数
```
$(addprefix <prefix>,<names...>) #把前缀<prefix>加到<names>中每个单词前面
$(addsuffix <suffix>,<names...>) #把后缀<suffix>加到<names>中每个单词后面
$(basename NAMES…)
	# 从文件名序列“NAMES…”中取出各个文件名的前缀部分
$(dir)
$(join)
$(notdir)		# 去除路径
$(suffix)
```
##### 其它函数
```
$(foreach <var>,<list>,<text>)
	# 将<list>中的单词依次代入变量<var>,进而计算出一系列<text>的值
$(if <condition>,<then-part>,[<else-part>])
	# 如果<condifion>为真，执行<then-part>,否则执行<else-part>
$(call <expression>,<parm1>,<parm2>,...)
	# 将参数代入<expression>，并返回其值。expression中的$1、$2分别对应着parm1、parm2
$(eval text)
	# text的内容将作为makefile的一部分而被make解析和执行
$(origin <variable>)
	# 返回变量variable的来源
$(shell command)		# 执行shell命令。可能影响系统性能，不建议使用。
```
##### 控制make的函数

```
$(error <text ...>)
$(warning >text ...>)
```



#### make的运行

##### 退出码

0成功，1错误，2尚不理解。

##### 指定Makefile

使用-f或-file参数

##### 指定目标

- all：所有目标的目标，其功能一般是编译所有的目标。
- clean：删除所有被make创建的文件。
- install：安装。
- print：列出改变过的源文件。
- tar：源程序打包。
- dist：文件压缩。
- TAGS：更新所有的目标。
- check和test：测试。
- '.PHONY'：指定伪目标，有的目标不是要生成文件，而只是一个标签。
- '.DEFAULT'：用于没有指定规则的目标。
- '.IGNORE'：将忽略执行步骤中的错误。

#### 隐含规则

	.SUFFIXES: .c .h .S 定义自己的后缀

##### 自动化变量

```
$@		# 目标文件集
$%		# 如果目标是归档文件，则表示规则中的目标成员名。否则，为空。
$<		# 第一个依赖文件
$?		# 所有比目标新的依赖文件。
$^		# 所有依赖文件的集合，会去除重复的目标
$+		# 所有依赖文件的集合，但不去除重复目标
$*		# 表示目标模式中%之前的部分。

$(<D)	# 依赖文件集里的第一个目录的名字
$(<F)	# 依赖文件集里的第一个文件的名字
```



#### 更新函数库文件

#### 参考资料

- [Makefile Conventions](http://www.gnu.org/prep/standards/html_node/Makefile-Conventions.html#Makefile-Conventions)

- [Makefiles](https://www.gnu.org/software/make/manual/html_node/Makefiles.html#Makefiles)

- [跟我一起写Makefile](http://wiki.ubuntu.com.cn/%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E5%86%99Makefile)