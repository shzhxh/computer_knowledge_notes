```
od [options] [files]	# 将文件内容以八进制或其它进制输出到标准输出
```

#### 选项

```
-t, --format=<tYPE>	# 选项输出格式
```

##### TYPE的定义

```
a
c
d[SIZE]
f[SIZE]
o[SIZE]
u[SIZE]
x[SIZE]	# 十六进制，每个整数代表[SIZE]个字节
	# [SIZE]里C代表sizeof(char),S,I,L,F,D,L.
```

