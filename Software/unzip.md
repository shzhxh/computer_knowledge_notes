#### 简介

解压zip文件的工具

#### 语法

```
unzip [选项]　file[.zip] [file...]
```

#### 选项

```

```

#### 示例

```
unzip -l "Weibo Data.zip"	# 显示"weibo Data.zip"所包含的文件
unzip "Weibo Data.zip"		# 解压"Weibo Data.zip"到当前文件夹
unzip src.zip -d /usr/src/	# 解压src.zip到目录/usr/src/
unzip -O cp936				# Windows下生成的zip文件在Linux下会乱码，需要使用此选项
```

