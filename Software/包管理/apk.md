#### 简介

Alpine的包管理器

```
apk [options] <command> [args]
```

#### 命令

##### 安装与删除

```bash
apk add
apk del
```

##### 系统管理

```bash
apk fix
apk update
apk upgrade
apk cache
```

##### 检索

```bash
apk info
apk list
apk dot
apk policy
```

##### 仓库管理

```
apk index
apk fetch
apk manifest	# 显示包里所包含文件的checksums
apk verify
```

##### 其它

```bash
apk audit
apk stats
apk version
```

#### 全局选项

```bash
-U, --update-cache	# 相当于‘--cache-max-age 1’，即缓存中的索引最长可存在1分钟
--cache-max-age <AGE>	# 缓存中的索引最长可以存在<AGE>分钟
--no-cache	# 不使用本地缓存
```



#### 提交选项