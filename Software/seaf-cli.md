```
seaf-cli <subcommand>
```

#### 子命令

```
init		# 初始化配置目录
	-c, --confdir <CONFDIR>	# 指定配置目录
	-d, --dir <DIR>			# 指定存放seafile数据的目录

start		# 开启ccnet和seafile守护进程
	-c, --confdir <cONFDIR>	# 指定配置目录

stop		# 停止ccnet和seafile守护进程
	-c, --confdir <cONFDIR>	# 指定配置目录

list		# 列出本地库
	-c, --confdir <CONFDIR>	# 指定配置目录

list-remote	# 列出远程库
	-c, --confdir <CONFDIR>	# 指定配置目录
	-s, --server <SERVER>	# 服务器的URL
	-u, --username <USER>	# 用户名
	-p, --password <PASSWORD>	# 用户密码

status		# 显示同步状态
	-c, --confdir <cONFDIR>	# 指定配置目录

download	# 从seafile服务器下载一个库，通过id
	-c, --confdir <CONFDIR>	# 指定配置目录
	-l, --library <LIB>		# 库的id
	-s, --server <SERVER>	# 服务器的URL
    -d, --dir <DIR>			# 存放库的目录
    -u, --username <USER>	# 用户名
    -p, --password <PASSWORD>	# 用户密码
    -e, --libpasswd <LIBPASSWD>	# 库的密码

download-by-name	# 通过名字来下载库

sync		# 同步一个库
	-c, --confdir <CONFDIR>	# 指定配置目录
	-l, --library <LIB>		# 库的id
	-s, --server <SERVER>	# 服务器的URL
	-u, --username <USER>	# 用户名
	-p, --password <PASSWORD>	# 用户密码
	-d, --dir <DIR>			# 本地目录
    -e, --libpasswd <LIBPASSWD>	# 库的密码	
	
desync		# 解除同步
	-c, --confdir <CONFDIR>	# 指定配置目录
	-d, --dir <DIR>			# 本地目录

create		# 创建一个库
	-n, --name <NAME>	# 库名称
	-t, --desc <DESC>	# 库描述
    -e, --libpasswd <LIBPASSWD>	# 库的密码
    -c, --confdir <CONFDIR>	# 指定配置目录
	-s, --server <SERVER>	# 服务器的URL
	-u, --username <USER>	# 用户名
	-p, --password <PASSWORD>	# 用户密码

config		# 配置seafile客户端
```

#### 错误解决

```
# 错误提示 ValueError: unknown url type
# 原因分析 url格式不正确
# 解决方法 url前加上http://
```

