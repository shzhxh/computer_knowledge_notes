打印或修改终端的属性

```
stty <-F DEV> [SETINGS]
stty <-F DEV> [-a | -g]	# 打印当前设置
```

#### 选项

```
-a, --all			# 以人类易读的方式打印设置
-g, --save			# 以stty易读的方式打印设置
-F, --file=<DEV>	# 指定设备。默认stdin
--help
--version
```

#### 特殊字符

```
discard <char>	# <char> will toggle discarding of output
lnext <char>	# <char> will enter the next character quoted
```

#### 特殊设置

```
cols, columns N	# 告诉内核终端有N列
ispeed N	# 设置输入速度为N
ospeed N	# 设置输出速度为N
rows N		# 告诉内核终端有N行
```

#### 控制设置

```
csN			# 设置字符大小为N个比特，N的范围5~8
```

#### 输入设置

```
[-]icrnl	# 把回车转换为换行
[-]ixon		# 开启XON/XOFF流控制
[-]ixoff	# 开启start/stop字符的发送
```

#### 输出设置

```
```

#### 本地设置

```
[-]crterase	# 把删除字符显示为backspace-space-backspace
[-]crtkill	# 按照echoprt和echoe的设置杀掉所有的行
[-]echo		# 显示输入字符
[-]echoe	# 等同于[-]crterase，即把删除字符显示为backspace-space-backspace
[-]echok	# 在kill字符后显示新行
[-]echoek	# 等同于[-]crtkill
[-]echoprt	# echo erased characters backward, between '\' and '/'
[-]icanon	# 开启特殊字符：erase, kill, werase, rprnt
[-]isig		# 开启特殊字符：中断，退出，挂起
```

#### 合并设置

```
```

