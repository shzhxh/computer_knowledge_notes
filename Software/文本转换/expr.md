表达式求值，结果输出到标准输出。

```
expr <expression>			# 表达式求值
expr <--help | --version>	# 显示帮助信息或版本信息
```

#### 表达式的形式

```
# 要用空格隔开每个项
# 特定字符前要加反斜杠
# 包含空格或特定字符的字符串要用引号括起来
ARG1 | ARG2		# 
ARG1 & ARG2		# 
ARG1 < ARG2		# 
ARG1 <= ARG2	# 
ARG1 = ARG2		# 
ARG1 != ARG2	# 
ARG1 >= ARG2	# 
ARG1 > ARG2		# 
ARG1 + ARG2		# 
ARG1 - ARG2		# 
ARG1 \* ARG2	# 乘法
ARG1 / ARG2		# 
ARG1 % ARG2		# 
STRING : REGEXP
match STRING REGEXP
substr STRING POS LENGTH	# 提示STRING中的子串
index STRING CHARS	# 获取CHARS在STRING中的位置
length STRING		# 计算STRING的长度
+ TOKEN
( EXPRESSION )
```

#### 退出状态

```
0	# 结果非null也非0
1	# 结果为null或者0
2	# 语法无效
3	# 发生错误
```

