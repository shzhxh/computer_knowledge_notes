#### 简介

打包与压缩文件

#### 语法

```
zip [options] [zipfile] [file...]	# 把file...压缩为zipfile
```

#### 选项

```
-m, --move					# 将指定文件放进zip压缩包里
-r, --recurse-paths			# 递归地遍历目录结构
-R, --recurse-patterns		# 从当前目录递归地遍历目录结构
```

#### 用法

```
zip homework.zip *	//将当前文件夹下所有文件打包成homework.zip文件
```

