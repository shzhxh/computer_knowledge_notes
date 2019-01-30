#### 语法

```
gdb [选项] prog			# 调试程序prog
gdb [选项] prog procID	# 调试正在运行的程序prog，procID是其进程号
gdb [选项] prog core		# 调试程序prog并指定core文件
```

#### 选项

#### 命令

##### 别名类

其它命令的别名

```
ni		# 单步追踪指令
rc
rni
rsi
si		# 精确地单步追踪指令
stepping
tp
tty
where
ws
```

##### 断点类

使程序停在特定的地方

```
awatch
break		
catch
clear
commands
delete
disable
dprintf
enable
ftrace
hbreak
ignore
rbreak
rwatch
save
skip
strace
tbreak
tcatch
thbreak
trace
watch
```

##### 数据类

观察数据

```
display		# 程序停止的时候打印出表达式的值
print		# 打印出表达式的值
set
undisplay	# 程序停止的时候不再打印出某些表达式的值
x /FMT ADDRESS			# 查看内存,ADDRESS是所要查看内存的表达式
	# FMT的格式是nfu
	# u表示一个地址单元的长度，可取b字节,h半字,w字,g八字节
	# f表示显示形式，可取x十六进制,d十进制,u无符号十进制,o八进制,t二进制,
		# a地址，i指令，c字符，f浮点，s字符串,z十六进制且在左边补0
	# n表示内存单元个数，如果是负数则会向前显示
```

##### 文件类

指定和检查文件

```
file	# 指定调试文件
list	# 列出指定的函数或命令
```

##### 内部的

维护命令

##### obscure

obscure特性

##### 运行类

运行程序

```
target		# 连接到一个目标(机器或进程)。
target core	# 使用一个core文件作为target
target exec	# 使用一个可执行文件作为target
target extended-remote	# 通过串口使用远程计算机
target record-btrace	# 收集控制流并提供执行历史
target record-core		# 
target record-full		#
target remote			# 通过串口使用远程计算机
	# 后面跟的参数是它连接到的串口设备
target sim	# 使用自带模拟器
target tfile	# 使用追踪文件作为一个target
```

##### 栈类

检查栈

##### 状态类

状态查询

```
info		# 显示各类信息
```

##### 支持类

支持工具

```
help
quit
```

##### 追踪点

不通过停止程序的方式来追踪程序的执行

##### 用户定义类

用户自定义的命令

#### 调试内核

##### 方法1(以调试ucore为例)

```
打开两个窗口，一个执行qemu-system-i386 -hda bin/ucore.img -S -s，另一个执行gdb obj/bootblock.o
target remote localhost:1234		// 与qemu建立远程连接
```

##### 方法2(以调试hyperkernel为例)

```
# 打开两个窗口
# 一个窗口执行如下操作
qemu-system-x86_64 -kernel o.x86_64/hv6.bin -S
# alt_ctrl_1是输出窗口，alt_ctrl_2是qemu控制台，切换到qemu控制台
gdbserver tcp::1234	# 看起来是把qemu作为服务端，用tcp协议连接

# 另一个窗口执行如下操作
gdb o.x86_64/hv6.elf
target remote localhost:1234	# 连接到qemu服务端
```

#### 错误与解决方法：

##### Register 3921 is not available

- 现象：设置断点的时候提示“Register 3921 is not available”
- 分析：从字面上来看它说3921寄存器失效，3921对应的16进制数是0xf51，在riscv里我没找到对应编号的寄存器。网上说3921解码后的寄存器编号是0xf10，我不清楚是怎么解码的。出现此问题的原因，也许和misa寄存器的Ｃ位有关(压缩位)，也许是gdb本身的设置问题。
- 解决方法：输入`set riscv use_compressed_breakpoints on`