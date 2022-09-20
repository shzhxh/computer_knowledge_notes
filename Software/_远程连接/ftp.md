```
ftp [options] [host [port]]
pftp [options] [host [port]]
```

#### 选项

#### 命令

```
open <host> [port]	# 连接到ftp服务器
```

#### 安装tftp服务进程

```
# 注：tftp是Trivial File Transfer Protocol（简单的文件传输协议）
# for debian
sudo ufw allow tftp	# 打开tftp端口
sudo apt install tftpd-hpa	# 安装tftp服务进程
	# 配置文件在/etc/default/tftpd-hpa
```

