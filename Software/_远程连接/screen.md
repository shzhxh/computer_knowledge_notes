#### 简介

使用VT100/ANSI终端模拟的屏幕管理器

#### 语法

```
screen [ -options ] [ cmd [ args ] ]
screen -r [[pid.]tty[.host]]
screen -r sessionowner/[[pid.]tty[.host]]
```

#### 选项

```
-d , -D [pid.tty.host]	# 脱离正在运行的会话。
-ls, -list [match]	# 打印出pid.tty.host的列表及其创建时间戳。
-r [[sessionowner\]<pid.tty.host>]	# 恢复一个脱离的会话。
-S <sessionname>	# 当创建一个新的会话时，将其命名为<sessionname>。当与-X联用时，用于指定要操作的会话。
-X	# 向会话发送操作命令。可以让-S选项指定会话。可以让-d或-r选项指定会话是attached还是dettached。
```

#### 默认键绑定

```
Ctrl+a k	# 销毁当前窗口。相当于kill命令
Ctrl+a x	# 锁定当前终端。相当于lockscreen命令
Ctrl+a z	# 挂起screen。相当于suspend命令
Ctrl+a \	# 关闭所有窗口并结束screen。相当于quit命令
```



#### 定制

```
kill	# 销毁当前窗口
quit	# 关闭所有的窗口并结束screen
```



#### 窗口类型

screen命令提供了三种窗口类型。第一个参数定义了窗口类型，所有的窗口类型都是正常类型的特殊情况。

- 正常窗口。包含了一个shell或在shell中运行的系统命令。
- 如果第一个参数是tty名(比如/dev/ttyUSB1)，那么窗口将与此设备直接连接。这个窗口类型类似于`screen cu -l /dev/ttyUSB1`。
- 如果第一个参数是`//telnet`，则第二个参数应该是主机名，第三个参数(可选)可能是个TCP端口号。这将使用telnet协议连接到远程主机。

#### 例子

```
screen /dev/ttyUSB0 115200	# 连接USB转串口设备，波特率为115200
```

