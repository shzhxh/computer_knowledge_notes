用于管理分配给文件的磁盘空间，包括预分配和释放。

由于预分配是分配块并将其标记为未初始化，不需要进行数据IO，这比给文件里填充0要快多了。

#### 语法

```
fallocate [-c | -p | -z] [-o offset] [-l length] [-n] <filename>
fallocate -d [-o offset] [-l length] <filename>
fallocate -x [-o offset] [-l length] <filename>
```

#### 选项

```
# 注：-c,-d,-p,-z是互斥的
-c
-d
-i
-l
-n
-o
-p
-v
-x
-z
```



#### 退出码

```
0	# 成功
1	# 失败
```

