```
git format-patch [options] <since>	# 生成补丁
git format-patch [options] <rivision range>	# 生成补丁
```

#### 示例

```
git format-patch HEAD^^^	# 生成最近的三个补丁
git format-patch -3			# 生成最近的三个补丁
```

