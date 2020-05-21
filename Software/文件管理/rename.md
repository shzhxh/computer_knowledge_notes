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
rename abc ABC abc*	# 把abc开头的文件里，abc修改为ABC(for C)
rename 's/abc/ABC/' abc*	# 把abc开头的文件里，abc修改为ABC(for Perl)
```

