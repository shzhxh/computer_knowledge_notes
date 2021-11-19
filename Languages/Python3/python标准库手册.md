[Python Standard Library](https://docs.python.org/3/library/)

> Python的中文化已经做的很好了，本文已无必要再看了。请直接看[Python标准库](https://docs.python.org/zh-cn/3/library/index.html)

#### 1. 简介

#### 4. 内建类型

##### 集合

```python
class set([iterable])	# 创建新集合。
```

#### 10. 函数式编程模块

#### 11. 文件与目录访问

```python
tempfile	# 生成临时文件与目录
	tempfile.NamedTemporaryFile()	# 创建一个临时文件。这个文件在关闭后默认被立即删除。
glob.glob(pathname,*,recursive=False)	# 查找与pathname匹配的路径，返回值是这些路径的列表。
shutil		# 高阶文件操作
	shtil.copyfileobj(fsrc, fdst[,length])
```

#### 16. 通用操作系统服务

```
argparse	# 命令行里选项、参数和子命令的解析器
	# 使用argparse的第一步：创建一个解析器
	argparse.ArgumentParser()	# 创建ArgumentParser对象
	# 使用argparse的第二步：添加参数
	ArgumentParser.add_argument()	# 定义参数的解析方法
		# name or flags：必有的参数，指选项(如-f, --foo)或参数(如bar)。
		# choices：指定当前选项可接受的参数列表。
		# const：指定一些常数，这些常数被action或nargs所使用。
		# action：指定当前选项相关联的动作。
			# 动作store：存储参数的值。默认动作。
			# 动作store_const：存储被const指定的值。
			# 动作store_true和store_false：分别用来存储True和False。
		# help：对当前选项的简短描述。
	# 使用argparse的第三步：解析参数
	parser.parse_args()
```



#### 17. 并行执行

```python
# subprocess模块用于创建和管理新进程
subprocess.run(args,...)	# 运行args
	# shell : True则通过shell执行
    # timeout : 设定超时的时间，单位为秒
    # check : True则退出异常时(返回非0)产生CalledProcessError异常。
```



#### 20. 结构化标记处理工具

#### 29. 运行时服务

`__main__`是顶层代码执行范围的名字。当从标准输入、脚本或交互界面读取时，模块内的变量`__name__`的值被设置为`__main__`。对于包，可以通过包含一个`__main__.py`的模块来实现同样的效果，当通过`-m`参数运行模块时`__main__.py`中的代码将被执行。

#### 37. 无正式文件的模块