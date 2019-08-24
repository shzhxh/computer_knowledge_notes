#### 简介

更改文件拥有者与属组

#### 语法

```
chown [options] [owner[:[group]]] <files>
chown [options] --reference=RFILE <files>
```

#### 选项

```
-R, --recursive	# 递归地作用于文件和文件夹
```



#### 示例

```bash
chown -R root /u	# 将目录/u的所有者递归地更改为root
```

