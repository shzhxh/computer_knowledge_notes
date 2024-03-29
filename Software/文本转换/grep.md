#### 语法

```
grep [OPTIONS] PATTERN [FILE...]		# 打印出能匹配正则表达式的行
grep [OPTIONS] -e PATTERN ... [FILE...]	# 指定正则表达式为PATTERN
grep [OPTIONS] -f FILE ... [FILE...]	# 正则表达式是从文件FILE中获取
```

```
egrep # 等价于grep -E
fgrep # 等价于grep -F
rgrep # 等价于grep -r
```

#### 选项

##### 普通程序信息

```
--help
-V, --version
```

##### 匹配器的选择

```
-E, --extended-regexp	# 把<PATTERN>理解为扩展的正则表达式
-F, --fixed-strings		# 把<PATTERN>理解为固定的字符串，而不是正则表达式
-G, --basic-regexp		# 把<PATTERN>理解为基本的正则表达式，这是默认选项
-P, --perl-regexp		# 把<PATTERN>理解为Perl兼容的正则表达式(PCRE)
```

关于PCRE，详见`man 3 pcresyntax`和`man 3 pcrepattern`

##### 匹配控制

```
-e PATTERN, --regexp=PATTERN	# 指定正则表达式
-f FILE, --file=FILE	# 从文件FILE中获取PATTERN，每行一个
-i, --ignre-case	# 忽略大小写
-v, --invert-match	# 不包含指定的字符串
-w, --word-regexp
-x, --line-regexp
-y
```

##### 普通输出控制

```
-c, --count	# 不进行正常输出，而只输出每个文件匹配的行数
-L, --files-without-match	# 不进行正常输出，而只打印不匹配指定样式的文件名
-l, --files-with-matches	# 不进行正常输出，而只打印匹配指定样式的文件名
-o, --only-matching			# 只打印每行中匹配的部分，且每个匹配的部分都作为一行输出
-q, --quiet, --silent		# 静默，不向标准输出写任何内容
```

##### 输出行前缀控制

```
-b, --byte-offset
-n, --line-number	# 在输出里显示行号
```

##### 文本行控制

```
-A NUM, --after-context=NUM		# 打印出匹配的行后面NUM行的内容。
-B NUM, --before-context=NUM	# 打印出匹配的行前面NUM行的内容。
-C NUM, -NUM, --context=NUM		# 打印出匹配的行前后NUM行的内容。
```

##### 文件与目录选择

```
-a, --text		# 等价于 --binary-files=test，像处理文本文件一样处理二进制文件
--binary-files=<TYPE>	# 
--exclude=<GLOB>	# 名称满足模式<GLOB>的文件将会被跳过
--exclude-from=<FILE>	# 要被忽略的文件名放在<FILE>里
--exclude-dir=<GLOB>	# 名称满足模式<GLOB>的目录将会被跳过
--include=<GLOB>		# 仅搜索文件名满足<GLOB>的文件 
-I		# 等价于 --binary-files=without-match, 不处理二进制文件
-r, --recursive	# 递归搜索，对于符号链接只有在命令行里指定了才会追踪它
-R, --dereference-recursive	# 递归搜索，会追踪所有的符号链接
```

##### 其它选项

```
--line-buffered
```



#### 正则表达式

详见“编译原理”目录下的“正则表达式.md”

#### 环境变量

#### 退出状态

```
0	# 至少一条匹配的结果
1	# 无匹配的结果
2	# 有错误发生
```

#### 存在的问题

1. `[^\n]`本应理解为不包含`\n`的所有字符，但grep似乎把它理解为不包含`\、n、 \n`这三个字符的所有字符。
