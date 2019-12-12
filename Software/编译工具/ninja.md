一个构建工具，它的作用和make命令是一样的。

#### ninja文件介绍

```
# 规则(rule)
rule <rule_name>
  command = gcc ...
  description = 
  
# build语句
build <target_name> : <rule_name> <inputs>

# subninja和include，都是用来引入其它的ninja文件。不同点是，subninja引入的子模块可以使用父模块的变量，include引入的子模块不可以使用交模块中的变量。
```

