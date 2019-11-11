```
jq [options] <filter> [files] # 处理json
```

#### filters

```
.	# 不加修改地把输入复制到输出
```

#### 选项

```
--arg a v	# 将变量$a的值设为v
-r	# 输出原始字符串(如不加此选项，默认输出json文本)
-s	# 把所有输入都读到数组里，并把filter作用于此数组
```

#### 参考

[jq 的使用介绍](https://www.ibm.com/developerworks/cn/linux/1612_chengg_jq/index.html)