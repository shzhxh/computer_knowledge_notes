#### 简介

收发邮件。

#### 语法

#### 选项

#### 示例

```
sudo apt install s-nail
sudo vim /etc/s-nail.rc		# 添加如下内容
	set from=user@163.com	# user为163邮箱的用户名
	set smtp=smtp.163.com	# 对于163邮箱来说固定为此域名
	set smtp-auth-user=user@163.com	# 与前面相同
	set smtp-auth-password=xxxx	# 授权码。需登陆163邮箱，在“设置->POP3/SMTP/IMAP”里指定
	set smtp-auth=login		# 固定值
echo "haha" | s-anil -s "test" xxx@qq.com	# 向xxx@qq.com发送邮件 
```

