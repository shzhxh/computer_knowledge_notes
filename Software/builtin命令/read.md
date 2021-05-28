```
read [options] [name ...]	# 读取一行，把它分成多个字段，这些字段会分配给[name ...]
```

#### 选项

```
-p <prompt>	# 在读取之前，先输出不带换行符的字符串<prompt>
```

#### 示例

```bash
cat file1 | while read line	# 从文件file1中依次读出每行的内容到变量line
```

