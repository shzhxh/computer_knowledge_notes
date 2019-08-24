[git log进阶](https://www.cnblogs.com/irocker/p/advanced-git-log.html)：git-log用法详细介绍

#### 简介

显示提交记录。

#### 语法

```
git log [options] [revision range] [path]
```

#### 选项

##### 通用

##### 提交的限制

```
-<number>, -n <number>, --max-count=<number>	# 查看最近number次提交
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

