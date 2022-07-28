#### 简介
通过串口与外部硬件交互的软件

#### 语法

```
minicom [options] [configuration]
```

#### 命令行

##### 选项

```
-b, --baudrate	# 指定波特率，这将覆盖配置文件里的值
-D, --device	# 指定设备，这将覆盖配置文件里的值
-h, --help
-v, --version
```



##### 配置

configuration默认为一个叫"minirc.dfl"的文件，如果给了minicom一个参数，它将默认使用叫"minirc.configuration"的文件。所以，可以给不同的串口、不同的用户创建配置文件。如果用户创建了自己的配置文件，它将在用户的home目录显示为".minirc.dfl"或".minirc.configuration"。

#### 用法

Ctrl+A，再按q，退出。

`minicom -D /dev/ttyUSB0`连接到串口ttyUSB0。

#### 问题解决

1. 无法输入字符

   > Ctrl-A + O进入配置，选串口设置，把"硬件流控制"设置为No，把“软件流控制”设置为Yes。然后"把设置保存为dfl"。
   
2. 字符都挤到了最右边

   原因分析：Windows认为`\r`是换行，Linux认为`\n`是换行。minicom默认为`\n`是换行，如果串口设备上的系统认为输出`\r`是换行，则显示异常。

   解决方法：`Ctrl+a z`，再输入`U`，选择`Add Carrier Ret`。