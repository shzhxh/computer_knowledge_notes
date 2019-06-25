#### 简介

编码或解码数据，并输出到标准输出。

数据按照RFC4648中的base32字母表进行编码。解码时，除了base32字母表中的字节外，输入中还可能包含新行，此时可以用`-i`选项来从编码流中的非base32字母表中的字母恢复。

#### 语法

```
base32 [options] [fle]
```

#### 选项

```
-d, --decode	# 解码
-i, --ignore-garbage	# 解码时忽略非base32字母表中的字符。
-w, --wrap=COLS
--help
--version
```

