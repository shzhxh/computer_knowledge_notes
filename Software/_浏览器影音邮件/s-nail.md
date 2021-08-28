#### 简介

收发邮件。

#### 语法

#### 选项

#### 示例

```
# 安装
sudo apt install s-nail
# 配置
sudo vim /etc/s-nail.rc		# 添加如下内容
	set from=user@163.com	# user为163邮箱的用户名
	set smtp=smtp.163.com	# 对于163邮箱来说固定为此域名
	set smtp-auth-user=user@163.com	# 与前面相同
	set smtp-auth-password=xxxx	# 授权码。需登陆163邮箱，在“设置->POP3/SMTP/IMAP”里指定
	set smtp-auth=login		# 固定值
# 发送邮件
echo "haha" | s-nail -s "test" xxx@qq.com	# 向xxx@qq.com发送邮件 
s-nail -s "test" xxx@qq.com < filename		# 将文件filename中的内容发送出去
s-nail -s "test" xxx@qq.com,xxx@163.com < abc	# 向多人发邮件
```

#### 错误解决

1. Please set *ssl-rand-file* to a file with sufficient entropy.

   之前还能正常发送邮件，某天开始就不行了。解决方法如下：

   ```
   sudo vim /etc/s-nail.rc
   	# 最后一行添加set ssl-rand-file=/tmp/random-file
   ```

   