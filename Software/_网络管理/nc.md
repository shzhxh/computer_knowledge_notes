#### 安装

```
pacman -Ss netcat
```

#### 用法

```
nc [options] [destination] [port]	# TCP和UDP的连接与侦听
```

#### 选项

```
-l	# 监听一个连接，而不是初始化到远程机器的连接。
-u	# 使用UDP。如不用此选项则使用TCP。
-v	# 输出详细信息
-w timeout	# 设置等待连线的时间为timeout秒
```



#### 示例

简单聊天工具

```
	nc -l 1234		//一台机器上监听1234端口
	nc remote_ip 1234	//另一台机器连接到监听了1234端口的机器
	//输入ctrl+D可以结束对话
```

端口扫描：

```
	nc -zv -w 2 remote_ip 20-30	//扫描对方机器的20-30端口是否开放,可以用-w指定超时时间
```



数据传输：
```
	nc -l 1234 > filename.out	//一台机器
	nc remote_ip 1234 < filename.in	//另一台机器
开启本机端口
	nc -p 1234 -w 5 remote_ip 42	//开启本机1234端口，连接到对方42端口，时延5秒
```