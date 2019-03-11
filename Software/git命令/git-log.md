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
--since=<date>, --after=<date>	# 显示date之后的提交
--until=<date>, --before=<date>	# 显示date之前的提交
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

#### 使用-P生成补丁

#### 合并DIFF格式

#### 使用示例

```
git log --after='7 days'	# 显示当前到7天前的提交
git log --before=2019-01-02	# 显示2019-01-02前的提交
```

