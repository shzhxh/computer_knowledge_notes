```
git format-patch [options] <since>	# 从某次提交开始生成补丁
git format-patch [options] <rivision range>	# 从两次提交之间生成补丁
```

#### 选项

```
-[k]	# 生成[k]个提交的补丁
```



#### 示例

```
git format-patch HEAD^^^	# 生成最近的三个补丁
git format-patch -3			# 生成最近的三个补丁
git format-patch -1 aaaa	# 仅生成提交aaaa的一个补丁
```

