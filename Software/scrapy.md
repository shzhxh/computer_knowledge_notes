#### 简介

scrapy是一个用python写的web抓取框架。

scrapy命令行工具。它提供了许多命令，每个命令都有自己的语法。

#### 语法

```
scrapy <命令> [选项] [参数]
```

#### 命令

##### bench

运行快速的基准测试

##### fetch

使用Scrapy下载器获取一个URL

##### genspider

使用预定义模板创建一个新爬虫

##### runspider

运行一个自包含的爬虫(不创建项目)

##### settings

获取设置的值

##### shell

交互式控制台

##### startproject

创建新项目

##### version

打印版本号

##### view

在浏览器中打开URL

#### 从项目目录运行的命令

##### crawl

```
scrapy crawl [option] <spider>	# 运行一个spider
	# 选项
	# -h, --help	显示crawl命令的帮助信息
	# -a NAME=VALUE	设置spider的参数
	# -o FILE, --oputput=FILE
	# -t FORMAT, --output-format=FORMAT
```

