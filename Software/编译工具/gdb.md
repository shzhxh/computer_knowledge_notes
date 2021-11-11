#### 语法

```
gdb [选项] prog			# 调试程序prog
gdb [选项] prog procID	# 调试正在运行的程序prog，procID是其进程号
gdb [选项] prog core		# 调试程序prog并指定core文件(core文件是异常退出后转储出来的文件)
```

#### 选项

```
-ex <cmd>	# 执行GDB命令<cmd>
```



#### 命令

##### 别名类

其它命令的别名

```
ni		# 即nexti，单步追踪指令
rc
rni
rsi
si		# 即stepi，精确地单步追踪指令
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
break	# 在特定位置设置断点。
	# b 10 : 断点在第10行
	# b main : 断点在main函数
	# b *0x001 : 断点在内存地址0x001处
catch
clear
commands
continue	# 程序在信号或断点处会停下来，此命令让程序继续执行。
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
set args	# 指定程序的参数
set riscv	# risc-v专有命令
set riscv use_compressed_breakpoints	# 设置是否使用压缩的断点
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

```
flushregs	# 强制刷新寄存器缓存
maintenance agent	# 把表达式转换为用于追踪的远程代理字节码
maintenance btrace	# 追踪分支(branch tracing)
maintenance check	# 检查gdb状态
maintenance cplus	# C++
maintenance demangler-warning	# demangler警告
maintenance deprecate	# Deprecate a command
maintenance dump-me	# 核心转储(dump core)
maintenance expand-symtabs	# 展开符号表
maintenance flush-symbol-cache	# 刷新符号缓存
maintenance info	# 显示被调试程序的内部信息
	program-spaces	# 打印gdb所管理的所有程序的内存空间信息
maintenance internal-error	# 产生一个内部错误
maintenance internal-warning	# 产生一个内部警告
maintenance packet	# 发送任意包到远程目标
maintenance print	# 打印内部状态
	statistics	# 对于程序中的每个目标文件，打印objfile和bcache的信息。
maintenance selftest	# 运行gdb的单元测试
maintenance set	# 设置内部变量
	per-command	# 设置每个命令的统计信息
	per-command space [on|off]	# 是否打印每个命令的内存使用
maintenance show	# 显示内部变量
maintenance space <value>	# 显示空间使用。相当于"maint set per-command space"，value为非0则开启此功能，value为0则关闭此功能。
maintenance time	# 显示时间使用
maintenance translate-address	# 把节名和地址转换为符号
maintenance undeprecate	#  Undeprecate a command
```



##### obscure

obscure特性

##### 运行类

运行程序

```
start		# 开始执行程序，在main处停止
starti		# 开始执行程序，在第一条指令处停止
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
info register(s)	# 显示整型计数器及它们的类型。若带参数则显示给定寄存器的信息。
```

##### 支持类

支持工具

```
!
add-auto-load-scripts-path
add-auto-load-scripts-directory
alias		# 为已有命令取别名
apropos		# 查找满足REGEXP的命令
help		# 打印出命令的帮助信息
quit		# 退出gdb
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