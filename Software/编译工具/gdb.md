#### 语法

```
gdb [选项] prog			# 调试程序prog
gdb [选项] prog procID	# 调试正在运行的程序prog，procID是其进程号
gdb [选项] prog core		# 调试程序prog并指定core文件
```

#### 选项

#### 命令

	file			//加载调试文件
	run			//运行被调试文件
	continue		//继续执行
	list			//列出代码
	x /nfu			//查看内存
		//u表示一个地址单元的长度，可取b,h,w,g
		//f表示显示形式，可取x,d,u,o,t,i指令，c字符，f浮点
		//n表示内存单元个数
	breakpoint		//设置断点
	delete			//删除断点
	step			//单步执行，进入
	next			//单步执行，不进入
	si/ni			//同s/n,不过针对的是汇编指令（s/n针对的是源代码）
	print			//显示变量的值
	display			//显示数据
	undisplay		//取消display设置
	info			//显示各类信息
	quit			//退出
	help			//帮助命令

#### 调试内核

##### 方法1(调试ucore)

```
打开两个窗口，一个执行qemu-system-i386 -hda bin/ucore.img -S -s，另一个执行gdb obj/bootblock.o
target remote localhost:1234		// 与qemu建立远程连接
```

##### 方法2(调试ucore)

```
打开两个窗口，一个执行make debug,另一个执行gdb

b bootmain.c:bootmain		// 在bootmain.c的bootmain函数下断点
p /x *(struct elfhdr *)0x10000;	// 在0x10000处按sttuct elfhdr结构显示其内容
info breakpoint			// 显示breakpoint的信息
```

##### 方法3(调试hyperkernel)

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

### 

#### 错误与解决方法：

##### Register 3921 is not available

- 现象：设置断点的时候提示“Register 3921 is not available”
- 分析：从字面上来看它说3921寄存器失效，3921对应的16进制数是0xf51，在riscv里我没找到对应编号的寄存器。网上说3921解码后的寄存器编号是0xf10，我不清楚是怎么解码的。出现此问题的原因，也许和misa寄存器的Ｃ位有关(压缩位)，也许是gdb本身的设置问题。
- 解决方法：输入`set riscv use_compressed_breakpoints on`