[git log进阶](https://www.cnblogs.com/irocker/p/advanced-git-log.html)：git-log用法详细介绍

#### 简介

显示提交记录。

#### 语法

```
git log [options] [revision range] [[--] paths]
	# <revision range>用于指定revision的范围。如不指定，则默认为HEAD。详见gitrevisions(7)的"Specifying Ranges"节。
		# r1..r2用于表示r1没有而r2有的提交。
		# r1...r2用于表示r1和r2之间的提交差异。即可以r1比r2早，也可以r2比r1早。
	# [--] paths用于指定要显示的路径
```

#### 选项

##### 通用

##### 提交的限制

```
-<number>, -n <number>, --max-count=<number>	# 查看最近number次提交
--pretty[=<fmt>], --format=<fmt>	# 用<fmt>格式美化输出。默认为medium。
	# <fmt>可选的形式：oneline, short, medium, full, fuller, reference, email, raw, format:<string> 和 tformat:<string>.
--since=<date>, --after=<date>					# 显示date之后的提交
--until=<date>, --before=<date>					# 显示date之前的提交
--author=<pattern>, --commiter=<pattern>		# 仅显示作者/提交者满足pattern模式的提交,可以多次使用此选项指定多个作者/提交者
--grep-reflog=<pattern>		# 仅匹配reflog入口满足pattern模式的提交
--grep=<pattern>			# 仅匹配log信息满足pattern模式的提交,如多次指定此选项表示满足任意一个pattern即可
--all-match					# 匹配满足所有--grep的提交,而不是满足任何一个即可.
```



##### 简化历史

##### 提交次序

##### 对象遍历

##### 提交格式

```
--pretty[=format], --format=<format>
--abbrev-commit
--oneline		# 等价于--pretty=oneline --abbrev-commit
```



##### 比较格式

#### PRETTY的格式

```
oneline : <hash> <title line>
medium :
	commit <hash>
	Author: <author>
	Date: <author date>
	<title line>
	<full commit message>
format:<string>
	<string>	# 其形式类似于printf里的fmt字符串，其中占位符的含义：
		# %n : 相当于printf里的\n
		# %% : 相当于printf里的%%
		# %H : 完整commit哈希
		# %h : 简略的commit哈希
	
```



#### 通用DIFF选项

```
-p, -u, --patch	# 生成补丁，见#生成补丁
--stat[=<width>[,<name-width>[,<count>]]]	# 生成diffstat。
```



#### 使用-P生成补丁

#### 合并DIFF格式

#### 使用示例

```
git log --after='7 days'	# 显示当前到7天前的提交
git log --before=2019-01-02	# 显示2019-01-02前的提交
```

