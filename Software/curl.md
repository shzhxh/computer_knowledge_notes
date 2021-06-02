#### 简介

Client URL，用来请求web服务器，是一个上传下载工具。

#### 语法

`curl [options] [URL...]`

#### 选项

```
-i, --include	# 显示http response的头信息和网页代码
-I, --head		# 仅显示头信息
-L, --location	# 有些网址是自动跳转的，使用此参数则会请求新跳转到的网址。
-o, --output <file>	# 默认是输出到标准输出，使用此选项则输出的文件file。
--trace <file>	# 将详细信息输出到file，将得到比-v选项更详细的信息。
--trace-ascii <file>	# 类似于--trace选项，但会放弃二进制信息仅输出ascii信息。
-s, --silent	# 静默模式。
-S, --show-error	# 与-s联用，当产生错误时会显示出错信息。
-v, --verbose	# 显示一次http通信的整个过程。
```



#### 参考

- [curl的用法指南](http://www.ruanyifeng.com/blog/2019/09/curl-reference.html)
- [curl初学者教程](http://www.ruanyifeng.com/blog/2011/09/curl.html)
- [curl cookbook](https://catonmat.net/cookbooks/curl)
- `man curl`