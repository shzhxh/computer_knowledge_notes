```
tcpdump [options] [expression]	# 转储网络中的数据
```

#### 选项

```
-e	# 打印出link level header。
-i , --interface=<interface>	# 指定要监听的接口。如设置为any，则意为监听所有的接口。
-n	# 直接显示地址、端口号的数字。如不用此选项，则显示的是名字。
-#, --number	# 在行首显示包的编号。
-v	# 输出详细信息。
-vv	# 输出详细信息，比-v更高。
-vvv	# 输出详细信息，比-vv更高。
-x	# 打印包的数据部分(16进制)。
-xx	# 打印包的数据部分和link vevel header(16进制)。
-X	# 打印包的数据部分(16进制，ASCII)。
-XX	# 打印包的数据部分和link vevel header(16进制，ASCII)。
```

#### 示例

```
```

#### 输出格式

##### timestamps

##### link level header

对于以太网，指的是MAC层的源地址、目的地址、协议、包的长度。

对于FDDI网络

对于令牌环网

对于802.11网络

对于SLIP链接

##### ARP/RAPR包

##### IPv4包

##### TCP包

##### UDP包

