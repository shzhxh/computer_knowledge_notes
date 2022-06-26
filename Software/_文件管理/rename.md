##### C语言版用法

```
rename [options] <expression> <replacement> <files>	# 对于files，把其名称里的expression替换为replacement
```

##### Perl语言版用法

```
rename [options] <perlexpr> [files]	# 对于files,使用规则perlexpr重命名
```



#### 示例

```
# C语言版
rename abc ABC abc*	# 把abc开头的文件里，abc修改为ABC
rename '' ABC *txt	# 给txt结尾的文件加上前缀ABC

# Perl语言版
rename 's/abc/ABC/' abc*	# 把abc开头的文件里，abc修改为ABC
```

