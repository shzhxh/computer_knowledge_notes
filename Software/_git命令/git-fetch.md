#### 简介

从其它仓库下载objects和refs

#### 语法

```
git fetch [options] [<repo> [refspec ...]]	# 从单个仓库获取分支
	# 如没有指定<repo>则默认为origin
git fetch [options] <group>		# 从多个仓库获取分支，这些仓库定义在配置文件的remotes.<group>
git fetch --multiple [options] [<repo | group> ...]	# 从多个<repo>和<group>获取分支
fit fetch --all [options]	# 从所有的remotes里fetch
```



#### 选项

```
--all	# 从所有的remotes里fetch
-a, --append
--depth
--deepen
--multiple	# 允许指定多个<repo>和<group>
-p, --prune	# 在fetching之前，删除所有和远程没有对应关系的引用。
--shallow-since
--shallow-exclude
--unshallow
--update-shallow
... ...
```

#### 示例

```
```

