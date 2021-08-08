[Python Standard Library](https://docs.python.org/3/library/)

#### 1. 简介

#### 4. 内建类型

##### 集合

```python
class set([iterable])	# 创建新集合。
```

#### 10. 函数式编程模块

#### 11. 文件与目录访问

```python
glob.glob(pathname,*,recursive=False)	# 查找与pathname匹配的路径，返回值是这些路径的列表。
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

#### 37. 无正式文件的模块s