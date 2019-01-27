#### 简介

jekyll是博客格式的静态网站生成器。

#### 用法

```
jekyll <subcommand> [options]
```

#### 命令

```
docs				# 用于浏览离线文档，前提是要安装gelyll-docs这个gem
import				# 将以前自己写的旧博客注入jekyll
b, build			# 构建网页源码
clean				# 清除网页源码
doctor, hyde		# 扫描网页源码，打印特定的失效警告
help				# 显示给定目录的帮助信息
new					# 在当前目录下创建网站结构
serve, server, s	# 开启本地服务，以便通过localhost:4000在本地预览最终效果
```



#### 全局选项

```
-s, --source [DIR]			# 指定源文件夹，默认为当前目录
-d, --destination [DIR]		# 指定目标文件夹，默认为当前目录下的_site文件夹
--safe						# 安全模式(禁用自定义插件)，默认不开启
-p, --plugins PLUGIN_DIR	#
--layouts DIR	# 布局文件夹，默认为当前目录下的_layouts文件夹
--profile		# Generate a Liquid rendering profile
-h, --help		# 打印帮助信息
-v, --version	# 打印版本信息
-t, --trace		# 当错误发生时显示完整的backtrace
```

#### 目录结构

##### _config.yml

配置文件。

```bash
## 全局配置　##
source: DIR		# 相当于--source DIR
destination: DIR	# 相当于--destination DIR
safe: BOOL		# 相当于--safe
exclude: [DIR, FILE]	# 不转换指定的文件和目录
include: [DIR, FILE]	# 强制转换指定的文件和目录
keep_files: [DIR, FILE]	# 直接复制对应的文件和目录到网页源码，不使用jekyll转换
timezone: TIMEZONE		# 设置时区
encoding: ENCODING		# 设置文件的编码

## 编译选项　##
## 服务选项 ##
baseurl: URL	# 网站的根路径

## 自定义webrick标题 ##
## 指定build时的环境 ##
## 头信息的默认值 ##
## 优先权　##
## 默认配置　##
## Liquid选项 ##
## Markdown选项 ##
## 增量更新 ##
```



##### _drafts

存放未发布的文章。可以使用`jekyll serve --drafts`命令查看未发布的文章。

##### _includes

共用的布局或文章，方便重用。可以使用`{% include file.ext %}`来包含本目录中的文件。

##### _layouts

文章的布局。

##### _posts

个人的文章。文件名必须要符合`yyyy-mm-dd-title.MARKUP`的格式。

##### _data

网站数据。

##### _site

生成的网页就放在这里。因为远程的仓库会自动生成网页，所以应把它放在`.gitignore`文件里。

##### index.html

##### 其它文件和目录

完全复制到网站源文件中。

#### 博客内容

##### 头信息

头信息必须在文件的开始部分，并且在两行三虚线之间。

```
layout	# 指定模板文件。模板文件不需要加后缀，且必须在_layouts目录下。
permalink	# 指定该页面的url地址。默认为yyyy/mm/dd/title.html。
published	# 设为false则不展示该页面。
data		# 用于覆盖文章名字中的日期，以保障文章排序的正确性。日期的格式为"yyyy-mm-dd hh:mm:ss +/-TTTT"，其中时分秒(hh:mm:ss)和时区(+/-TTTT)是可选的。
category, categories	# 设置文章分类的属性，这样就可以依据分类来阅读文章。
tags		# 设置标签。
```

