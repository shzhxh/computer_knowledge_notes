#### 简介

用于远程登陆

#### 用法

```
ssh [选项] [user@]hostname [command]		# 连接到服务器hostname
```

#### 选项

##### 不带参数

```
-4		# 强制使用IPv4地址
-6		# 强制使用IPv6地址
-A
-a
-C		# 压缩数据传输
-f		# 后台认证用户/密码，通常和-N连用，不用登录到远程主机。
-G
-g		# 在-L/-R/-D参数中，允许远程主机连接到建立的转发的端口，如果不加这个参数，只允许本地主机建立连接。
-K
-k
-M
-N		# 不执行脚本或命令，通常与-f连用。
-n
-q
-s
-T
-t
-V
-v
-X
-x
-Y
-y
```

##### 带参数

```
-b bind_address
-c cipher_spec
-D [bind_address:]port	# 动态转发，即socks代理
-E log_file
-e escape_char
-F configfile
-I pkcs11
-i identit-J [user@]host[:port]y_file
-L address				# 本地转发
-l login_name
-m mac_spec
-O ctl_cmd
-o option
-p port					# 使用服务器的port端口
-Q query_option
-R address				# 远程转发
-S ctl_path
-W host:port
-w local_tun[:remote_tun]
```

#### 用法示例

```
# 本地转发
ssh -C -f -N -g -L <本地监听ip>:<本地端口>:<远程ip>:<远程端口> 用户名@目标IP -p <ssh端口>

# 远程转发
ssh -C -f -N -g -R <远程ip>:<远程端口>:<本地ip>:<本地端口> 用户名@目标IP -p <ssh端口>

# 动态转发
ssh -C -f -N -g -D <本地ip>:<本地端口> 用户名@目标IP -p <ssh端口>

# 免密登陆
ssh-keygen -t rsa	# 生成密钥对
ssh-copy-id -i ~/.ssh/id_rsa.pub user@ip	# 将公钥传输到远程机器
	# 可以在远程机器的.ssh/authorized_keys看到刚才添加的公钥
	
# 设置别名
cd .ssh && touch config	# 创建config文件
	# Host为别名，HostName为主机地址，User为登陆用户名，Port为端口号，IndentitiesOnly yes为固有配置
```

