#### 简介

从文件名中删除目录和后缀。会删除名称中的所有先导目录。如指定的话，也可删除后面的后缀。

注：如果要留下的是目录则使用`dirname`命令。

#### 语法

```
basename NAME [SUFFIX]
basename [OPTIONS] NAME
```

#### 选项

```
-a, --multiple
-s, --suffix=SUFFIX		# 删除指定后缀SUFFIX
-z, --zero				# 在输出行的末尾不换行
```

