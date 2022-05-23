#### 简介

Alpine的包管理器

```
apk [options] <command> [args]
```

#### 命令

##### 安装与删除

```bash
apk add	# 安装包
apk del	# 删除包
```

##### 系统管理

```bash
apk fix		# 修复、重新安装或更新包
apk update	# 更新仓库索引
apk upgrade	# 安装更新
apk cache
```

##### 检索

```bash
apk info	# 显示包的详情
apk list	# 列出匹配的包
apk dot
apk policy
apt search	# 查找包
```

##### 仓库管理

```shell
apk index
apk fetch		# 把包下载到一个本地目录
apk manifest	# 显示包里所包含的文件及其校验和
apk verify		# 验证包的完整性和签名
```

##### 其它

```bash
apk audit
apk stats		# 显示仓库和安装的统计信息
apk version
```

#### 全局选项

```bash
-U, --update-cache	# 相当于‘--cache-max-age 1’，即缓存中的索引最长可存在1分钟
--cache-max-age <AGE>	# 缓存中的索引最长可以存在<AGE>分钟
--no-cache	# 不使用本地缓存
```



#### 提交选项

#### repositories文件

在`/etc/apk/repositories`

#### 示例

```
apk add mandoc man-pages	# 安装用户手册
apk serch apk | grep doc	# 查找缺失的用户手册(比如apk的用户手册)
```

