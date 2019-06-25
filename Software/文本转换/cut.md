#### 简介

选取每行中的指定内容到标准输出

#### 语法

```
cut <-b | -c | -f> [options] [files]
```

#### 选项

```
-b, --bytes=LIST		# 选取指定字节
-c, --characters=LIST	# 选取指定字符
-f, --fields=LIST		# 选取指定的域
-d, --delimiter=DELIM	# -f选项默认用TAB为分割符，使用此选项可指定DELIM为分割符
-s, --only-delimited	# -f选项会打印不包含分割符的行，使用此选项则不打印不包含分割符的行

# LIST由若干range组成，每个range用逗号隔开，rang可以是如下形式：
  N		# N个字节、字符或域，N为正整数
  N-	# 从N开始至行尾
  N-M	# 从N到M
  -M	# 从行首到M
```

