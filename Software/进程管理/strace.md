#### 简介

追踪系统调用和信号(signals)

```bash
strace [options] <command> [args]
# 在最简单的情况下，strace运行指定的命令直到它退出。它会记录进程使用的系统调用和接收的信号。结果默认输出到标准错误，也可用-o选项输出到文件中。
# 注：选项不能写在<command>后面，否则会被认为是<command>的参数，导致选项无效。
# 输出的格式：
#   系统调用名 + 传递的参数 + “=” + 返回值
#   如果返回-1(发生了错误)，还会附加错误符号和错误信息
#   如果是信号，则会打印出信号符号和siginfo结构体
#   如果是未知的系统调用，则显示为16进制的形式。
#   系统调用的参数里如果有字符指针，则会解引用为字符串。字符串太长则用省略号表示 。
#   系统调用的参数里，简单的指针(如数组)用方括号标注，复杂的指针(如结构体)用花括号标注。
#   系统调用里的简单指针，数组用逗号分隔元素，位集(bit-sets)用空格分隔元素。
#   ~[]表示位集里的元素全都设置了，这样比较省空间。
```

#### 选项

##### 输出格式

```bash
-i	# 打印出系统调用的指针。
-o <filename>	# 把输出写入文件filename，而不是写到stderr. 
-t	# 打印出系统调用的时间，精确到秒。
-tt	# 打印出系统调用的时间，精确到毫秒。
-ttt	# 打印出epoch以来的秒数+系统调用的时间(精确到毫秒)。
-T	# 打印系统调用所花费的时间。
```



##### 统计

```bash
-c	# 统计每个系统调用的时间、调用次数和错误次数。
-S <sortby>	# 对输出的结果排序。可选参数有time,calls,name,nothing，默认time.
```



##### 过滤

```bash
-e <expr>	# 用expr来定义要追踪的事件及追踪它的方式。
	# expr的格式：[qualifier=][!][?]<value1>[,[?]value2...]
-e trace=<set>	# 指定要追踪的系统调用。默认trace=all。
	# set指系统调用的集合，如：open,close,read,write
-e trace=%file	# 追踪以文件名为参数的系统调用
-e trace=%process	# 追踪进程管理的系统调用
-e trace=%network	# 追踪网络相关的系统调用
-e trace=%signal	# 追踪信号相关的系统调用
-e trace=%ipc		# 追踪IPC相关的系统调用
-v	# 打印出系统调用参数的详细信息。
```



##### 追踪

```bash
-f	# 仅追踪子进程。这些子进程就是要追踪的进程，它们一般由fork,vfork或clone系统调用创建。
```



##### 启动

```bash
-p <pid>	# 把pid附加到进程。
```



##### 其它

```bash
-d	# 显示一些调试信息。
-V	# 打印版本号。
```

#### 错误记录

##### 1

- 错误描述：docker里运行strace，提示`ptrace(PTRACE_TRACEME, ...): Function not implemented`
- 原因分析：请参考[why-strace-doesnt-work-in-docker](https://jvns.ca/blog/2020/04/29/why-strace-doesnt-work-in-docker/)
- 解决方法：`docker run --cap-add=SYS_PTRACE  -it ubuntu:18.04 /bin/bash`

##### 2

- 错误描述：shell脚本里运行strace，提示`strace: exec: Permission denied`
- 原因分析：不知道为什么
- 解决方法：`bash ./script.sh`
