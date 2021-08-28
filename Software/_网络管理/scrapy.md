#### 简介

scrapy是一个用python写的web抓取框架。

scrapy命令行工具。它提供了许多命令，每个命令都有自己的语法。

##### 数据流向

1. Spider将URL发给Engine
2. Engine将request发给Scheduler
3. Scheduler创建request队列，并将request队列发给Engine
4. Engine将request队列发给Downloader
5. Downloader进行下载，并将下载结果交给Engine
6. Engine将下载结果交给Spider
7. Spider处理下载结果，生成item数据，并将item数据传给Engine
8. Engine将item数据传给Item Pipeline
9. Item Pipeline处理item数据

##### 制作爬虫的流程

1. 使用`scrapy startproject xxx`新建项目
2. 编写`items.py`明确要抓取的目录
3. 编写`spiders/xxspider.py`制作爬虫
4. 编写`pipelines.py`设计管道以存储抓取的内容

#### 语法

```
scrapy <命令> [选项] [参数]
```

#### 全局选项

```
--logfine=FILE
--loglevel=LEVEL, -L LEVEL
--nolog
--profile=FILE
--pidfile=FILE
--set=NAME=VALUE, -s NAME=VALUE
--pdb
 -h, --help	# 显示关于command的帮助信息
```



#### 命令

##### bench

运行快速的基准测试

##### fetch

使用Scrapy下载器获取一个URL

##### genspider

使用预定义模板创建一个新爬虫

```
spcrapy genspider [optins] <name> <domain>
# 选项
  -l, --list	# 列出所有模板
  -e, --edit	# 创建后即开始编辑spider
  -d TEMPLATE, --dump=TEMPLATE		# 将模板输出到标准输出
  -t TEMPLATE, --tmplate=TEMPLATE	# 使用自定义模板
  --force		# 如spider已存在，直接用模板覆盖它
```

##### runspider

运行一个自包含的爬虫(不创建项目)

##### settings

获取设置的值

##### shell

交互式控制台

##### startproject

创建新项目

```
scrapy startproject <project_name> [project_dir]

# 运行scrapy startproject mySpider后，将创建mySpider目录，其结构如下：
+ mySpider/
 -- scrapy.cfg			# 项目的配置文件
 -+ mySpider/			# 项目的Python模块
  ---  __init__.py
  ---  items.py			# 项目的目标文件
  ---  pipelines.py		# 项目的管道文件
  ---  settings.py		# 项目的设置文件
  --+  spiders/			# 爬虫所在的目录
    ---  __init__.py
    ---  ...
```



##### version

打印版本号

##### view

在浏览器中打开URL

#### 从项目目录运行的命令

##### crawl

```
scrapy crawl [option] <spider>	# 运行一个spider
# 选项
  -a NAME=VALUE					# 设置spider的参数
  -o FILE, --oputput=FILE		# 将抓取到的items保存到文件(支持的格式有：json, jsonlines, jl, csv, xml, marshal, pickle)
  -t FORMAT, --output-format=FORMAT	# 使用-o保存items时的格式
```

