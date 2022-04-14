#### 简介

将文件连接起来，然后打印到标准输出。cat这个名字应该是从concatenate这个单词来的。

#### 语法

```
cat [options] [files]
```

#### 选项

```
-A, --show-all	# 等价于-vET
-b	# 给非空的行编号，会覆盖-n选项
-e	# 等价于-vE
-E	# 在行尾打印$
-n	# 给所有行编号
-s	# 忽略重复的空行
-t	# 等价于-vT
-T	# 把TAB字符显示为^I
-u	
-v	# 除LFD和TAB外，使用^和M-表示法(LFD的意思是line feed，换行)
--help
--version
```

