#### 用法

```
kill [options] <pids>	# 给进程发送一个信号，默认为TERM
	# 负数的进程号表示进程组，即ps命令输出的PGID列
	# pid为-1是特殊的，它表示除kill进程自身和init进程之外的所有进程
```

#### 选项

```
-<signal>, -s <signal>, -signal <signal>	# 定义要发送的信号
-l, --list [signal	# 列出信号名
-L, --table	# 以列表的形式列出信号名
```

#### 信号

```
# 可以用"man 7 signal"命令查看关于信号的详细信息
# 常用的信号有：HUP, INT, KILL, STOP, CONT, 0
```

