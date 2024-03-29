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
-i <identity_file>
-J [user@]host[:port]y_file
-L [bind_address:]<port>:<host>:<hostport>				# 本地转发
-L [bind_address:]<port>:<remote_socket>
-L <local_socket>:<host>:<hostport>
-L <local_socket>:<remote_socket>
-l login_name
-m mac_spec
-O ctl_cmd
-o option
-p port					# 使用服务器的port端口
-Q query_option
-R [bind_address:]<port>:<host>:<hostport>				# 远程转发
-R [bind_address:]<port>:<local_socket>
-R <remote_socket>:<host>:<hostport>
-R <remote_socket>:<local_socket>
-R [bind_address:]<port>
-S ctl_path
-W host:port
-w local_tun[:remote_tun]
```

#### 用法示例

```
# 本地转发
ssh -fgNCL <本地监听ip>:<本地端口>:<远程ip>:<远程端口> 用户名@目标IP -p <ssh端口>

# 远程转发：本地通过<用户名@目标IP>转发到远程，这样远程用户通过<远程端口>就可以登陆到本地了。
ssh -fgNCR <远程ip>:<远程端口>:<本地ip>:<本地端口> 用户名@目标IP -p <ssh端口>

# 动态转发
ssh -fgNCD <本地ip>:<本地端口> 用户名@目标IP -p <ssh端口>

# 免密登陆
ssh-keygen -t rsa	# 生成密钥对
ssh-copy-id -i ~/.ssh/id_rsa.pub user@ip	# 将公钥传输到远程机器
	# 可以在远程机器的.ssh/authorized_keys看到刚才添加的公钥
	
# 设置别名
cd .ssh && touch config	# 创建config文件
	# Host为别名，HostName为主机地址，User为登陆用户名，Port为端口号，IdentitiesOnly yes为固有配置
	# 注：如遇到Bad owner or permissions on XXX/config，要把config修改为600权限。
```

#### 错误解决

1. 提示对方机器"Permission denied"

   > 查看对方机器的/etc/ssh/sshd_config文件：
   >
   > 如允许密码登陆，则“PasswordAuthentication yes”不能注释
   >
   > 如允许root登陆，则“PermitRootLogin yes”不能注释
   >
   > 如允许使用密钥登陆，则“PubkeyAuthentication yes”和“AuthorizedKeysFile .ssh/authorized_keys”不能注释

2. 提示对方机器"Connection refused"

   > 经查是目标机器没有运行sshd，故sudo apt install openssh-server

3. WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

   > 清空~/.ssh/know_hosts文件
