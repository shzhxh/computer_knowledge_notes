#### 简介

[scons官网](https://www.scons.org/)

使用Python语言作为配置文件的软件建造工具。会在当前目录下依次寻找SConstruct, Sconstruct, sconstruct作为配置文件，也可以用`-f`选项指定配置文件的文件名。可以通过scons用户手册，scons设计文档和scons源代码获取更多信息。

#### 语法

```
scons [options...] [name=val...] [targets...]
```

#### 选项:192

```
-b
-c, --clean, --remove	# 执行清理任务
-C
-D
-n, --just-print, --dry-run, --recon	# 只打印出要执行的命令，而不执行它。
-Q	# quiet的缩写，不打印冗余信息，但仍打印编译过程。
```

#### 配置文件:1083

##### 配置环境

```

```

##### 编译器方法:2082

```
Library(), env.Library()	# 等价于StaticLibrary()，编译成静态链接库
Object(), env.Object()		# 等价于StaticObject()，编译成目标文件
Program(), env.Program()	# 编译成可执行程序
SharedLibrary(), env.SharedLibrary()	# 编译成动态链接库
```



##### 干活的方法与函数:3862

##### 变量:6013

```
CCFLAGS	# 传递给编译器的编译选项
LIBPATH	# 链接库的搜索路径
LIBS	# 指明在链接过程中使用的库

```



##### 配置上下文:9953

##### 命令行构建变量:10327

##### 文件和目录结点:10565

#### 扩展SCONS:10592

##### 编译对象

##### 活动对象

##### 其它活动功能

##### 变量替换

##### 扫描对象

#### 系统定义行为

#### 例子

#### 环境