#### 描述

python的三个特色：解释型的语言，面向对象，简洁的语法。

#### 语法

```
python [选项] [-c command | script | -] [arguments]
  # python还可以执行目录和zip文件，这将从__main__.py开始执行
```

#### 选项

```
-c command			# 指定要执行的command命令。此选项将处于python命令的最末端，此选项之后的部分都将作为command的参数。
-m <module-name>	# 从sys.path里查找<module-name>，并运行对应的脚本<module-name>.py
```

#### 解释程序的接口

解释程序的接口就像一个UNIX Shell：

- 如果调用的是连接到tty设备的标准输入，它等待并执行命令直到读取到EOF。
- 如果调用的是文件或作为标准输入的文件，它将此文件作为脚本执行。
- 如果调用的是`-c command`，它将执行command。

#### 示例

```
python -m pydoc -p 0	# 查看本机安装的模块的文档
	# pydoc模块用于从模块生成文档。-p是指定pydoc模块HTTP服务的端口，如指定0则意思是从未使用的端口任选一个。
```

