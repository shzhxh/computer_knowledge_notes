```
iconv [options] [files]	# 把文本从一种字符编码轮换到另一种字符编码。如不指定[files]或指定的是破折号(-)，则从标准输入读取文本。如不使用-o选项，则写到标准输出。如不使用-f选项，则使用当前locale的字符编码。如不使用-t选项，则使用当前locale的字符编码。
```

#### 选项

```
-f, --from-code=<from-encoding>	# 指定输入字符的字符编码
-t, --to-code=<to-encoding>		# 指定输出字符的字符编码
-l, --list
-c
-o,  --output=<outputfile>		# 输出到<outputfile>
-s, --silent
--verbose
```

