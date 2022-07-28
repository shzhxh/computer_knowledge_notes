#### 简介

打包与压缩文件

#### 语法

```
zip [options] [zipfile] [file...]	# 把file...压缩为zipfile
```

#### 选项

```
-e, --encrypt	# 加密zip文件
-m, --move					# 将指定文件放进zip压缩包里
-P, --password <pwd>		# 使用<pwd>加密zip文件。但尽量不要使用此选项，因为明文的密码基本没有安全性可言。
-r, --recurse-paths			# 递归地遍历目录结构
-R, --recurse-patterns		# 从当前目录递归地遍历目录结构
-x, --exclude <files>		# 不包含满足规则<files>的文件
```

#### 用法

```
zip homework.zip *	//将当前文件夹下所有文件打包成homework.zip文件
zip -e abc.zip abc	// 将文件abc压缩为abc.zip,且进行加密
zip -r aaa.zip aaa	// 将目录aaa压缩为aaa.zip
```

