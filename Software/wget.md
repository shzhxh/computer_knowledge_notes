#### 简介

#### 语法

```
wget [option] [url]
```

#### 选项

##### 基本启动

```bash
-e ,--execute <cmd>	# 执行可以定义在文件.wgetrc里的命令。
	# 可能的cmd（详见"info wget"第6.3章： Wgetrc Commands）
	# http_proxy = <STRING>	# 使用STRING作为HTTP代理
	# https_proxy = <STR>	# 使用STR作为HTTP代理
	# use_proxy = <on/off>	# 是否使用代理
```



##### 日志和输入文件

```
-i, --input-file=<file>	# 从<file>中读取URLs。要求文件中每行一个URL。
-q, --quiet		# 关闭wget的输出
```



##### 下载

```
-c, --continue	# 继续获取下载到一半的文件。
-T <sec>, --timeout=<sec>	# 设置网络的timeout为<sec>秒。
-t <num>, --tries=<num>		# 尝试的次数设置为<num>。
-O <file>, --output-document=<file>	# 文档不会写入到相应的文件，而是把所有的文档连接起来写入到<file>。
```



##### 目录

##### HTTP

##### HTTPS

##### FTP

##### FTPS

##### 递归搜索

```
-r, --recursive	# 开启递归搜索，默认深度为5.
-k, --convert-links	# 下载完成后转换文档中的链接使其适合本地观看。
-p, --page-requisites	# 下载所有必需的文件，以正确显示给定HTML页。
```



##### 递归接收/拒绝

```
-np, --no-parent	# 不向父目录的方向递归检索。
```

#### 示例

```bash
wget -r -p -np -k <url>	# 批量下载url下的所有网页
```

