#### 条件表达式

```
# 文件比较
-f文件存在, -d目录存在, -e,文件或目录存在，-s文件存在且非空, -r文件存在且可读, -w, -x

# 字符串比较
==, =, !=, < , >, -n字符串非空, -z字符串为空

# 数字比较
-eq, -ne, -lt, -le, -gt, -ge

# 逻辑运算
-a且，-o或，&&且, ||或
```

#### 参考资料

1. [bash手册](http://www.gnu.org/software/bash/manual/bash.html)
2. `man bash`