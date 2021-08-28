#### 简介

使用VT100/ANSI终端模拟的屏幕管理器

#### 语法

```
screen [ -options ] [ cmd [ args ] ]
screen -r [[pid.]tty[.host]]
screen -r sessionowner/[[pid.]tty[.host]]
```



#### 窗口类型

screen命令提供了三种窗口类型。第一个参数定义了窗口类型，所有的窗口类型都是正常类型的特殊情况。

- 正常窗口。包含了一个shell或在shell中运行的系统命令。
- 如果第一个参数是tty名(比如/dev/ttyUSB1)，那么窗口将与此设备直接连接。这个窗口类型类似于`screen cu -l /dev/ttyUSB1`。
- 如果第一个参数是`//telnet`，则第二个参数应该是主机名，第三个参数(可选)可能是个TCP端口号。这将使用telnet协议连接到远程主机。