#### 简介

管理标签，包括创建、列出、删除或验证操作。一个标签对象是用GPG签名的。

#### 语法

```bash
git tag	<-a | -s | -u keyid> <tagname>	# 创建标签
	# 要创建tagname不能已经存在，除非带-f选项。
	# 会启动编辑器要求用户输入tag信息，除非使用-m msg或-F file选项。
	
git tag -d <tagnames>			# 删除标签
git tag -l						# 列出标签
git tag -v <tagnames>			# 验证标签
	
git push origin tag	# 将标签推送到服务器
```

#### 选项

```
-a, --annotate	# 创建一个未签名的，带注释的标签
-s, --sign		# 创建一个GPG签名的标签，以默认电邮地址作为key
-u, --local-user=<keyid>	# 创建一个GPG签名的标签，需自己指定key
-f, --force		# 强制替换一个已存在的标签
-d, --delete	# 删除标签
-v, --verify	# 验证标签的GPG签名
-l, --list		# 列出标签
-m, --message=<msg>			# 在msg里给出tag信息
-F, --file=<file>			# 在file里给出tag信息
```

