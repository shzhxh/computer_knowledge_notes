#### 用法

```
mkdir [option] directorys	# 创建目录
```



#### 选项

```
-m, --mode=MODE		# 设置文件的mode，就像chmod命令一样
-p, --parents		# 如果父文件夹不存在则直接创建
-v, --verbose		# 每创建一个文件夹都会输出一条信息
-Z					# 将每个创建的目录的SELinux安全上下文设置为默认类型。
--context[=CTX]		# 类似于-Z，或者如果指定CTX，则将SELinux或SMACK安全上下文设置为CTX。
```

