```
tr [options] <set1> [set2]	# 转换或删除字符
```

#### 选项

```
-c, -C, --complement
-d, --delete			# 删除标准输入中由SET1指定的字符，默认是转换
-s, --squeeze-repeats	# 
-t, --truncate-set1		# 首先把SET1截断到和SET2一样的长度
```

#### 关于SET

```
\NNN	# 字符的八进制值(1到3位)
\\		# 反斜杠
\a		# 警告
\b		# 退格
\f		# 换页
\n		# 换行
\r		# 回车
\t		# tab
\v		# 垂直制表键
CHAR1-CHAR2		# CHAR1和CHAR2之间的所有字符
[CHAR*]			# 只用在SET2末尾，用CHAR填充剩余字符串，直到和SET1的字符串等长
[CHAR*REPEAT]	# 重复CHAR字符REPEAT次。REPEAT以0开头为八进制，否则为十进制
[:alnum:]		# 所有字母和数字
[:alpha:]		# 所有字母
[:blank:]		# 所有水平空格
[:cntrl:]		# 所有控制字符
[:digit:]		# 所有数字
[:graph:]		# 所有可控制字符，不包括空白
[:lower:]		# 所有小写字母
[:print:]		# 所有可控制字符，包括空白
[:punct:]		# 
[:space:]		# 所有水平和垂直空格
[:upper:]		# 所有大写字母
[:xdigit:]		# 所有十六进制数字
[=CHAR=]		# 与CHAR相等的所有字符
```

