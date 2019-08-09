#### 简介

以标准输入为参数执行指定命令，若不指定命令则默认执行`echo`命令。可以使不支持管道的命令接收到从管道传来的参数。

#### 用法

```
xargs [options] [command [initial-arguments]]
```

#### 选项

```
-0, --null			# 把null做为分割符。find命令的-print0选项适用于此模式。
-d <delim>, --delimiter=<delim>	# 指定<delim>为分割符，默认分割符是空格和换行符
-I <replace-str>	# 指定每项参数的替代字符串为replace-str。
-L <max-lines>		# 指定命令接收的行数为max-lines。命令会多次执行直到把标准输入都读完。
-n <max-args>, --max-args=<max-args>	# 指定命令在每行接收的参数为max-args。
-p, --interactive	# 打印出最终要执行的命令，需要用户确认才可执行。
-P <max-procs>, --max-procs=<max-procs>	# 指定执行命令的最大进程数。如max-procs设为0则会开尽可能多的进程。max-procs默认值为1。
	# 注：-P应与-n或-L联用，这样命令才有并行执行的机会。
-t, --verbose		# 在执行命令前打印出要执行的命令。
```



#### 示例

```
# xargs不连接管道，单独使用的例子
xargs find -name	# 等待标准输入，搜索文件
"*txt"				# 要搜索的文件为以txt结尾的文件，按ctrl+d结束标准输入
```



#### 参考

- `man 1 xargs`
- [xargs命令教程 - 阮一峰](http://www.ruanyifeng.com/blog/2019/08/xargs-tutorial.html)