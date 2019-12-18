一个构建工具，它的作用和make命令是一样的。

整个构建过程可以看作一颗树，从叶子结点到根结点的过程。两个相连的结点就是input和output(或者叫target)，rule就是从input到output的方法，rule、input和output加起来就是edge（即build语句），一组rule或edge组成pool，scope是变量的作用域。

#### ninja文件介绍

```
# 变量有两种：内置与自定义
# 顶层的内置变量：
builddir : 指定一些文件的输出目录
ninja_required_version : 指定ninja命令的最低版本

# pool:定义一个pool
depth指并行的个数，pool里通过定义depth来控制并行的个数。
# 目前仅有一个内置的pool，名为console。这个pool的depth为1，可以直接访问stdin、stdout、stderror。

# default：指定默认的target，可以是多个

# phony：指定非文件的target

# 规则(rule)
rule <rule_name>
  command = gcc ...
  description = 
#rule的内置变量
command, description, generator, in, in_newline, out, depfile, deps, msvc_deps_prefix, restat, rspfile, rspfile_content
  
# build语句
build <target_name> : <rule_name> <inputs>
# 其中<target_name>和<inputs>可以是一个列表，第一部分不带任何符号是显示(explicit)的输出或输入；第二部分以"|"开头，是隐式(implicit)的输出或输入；第三部分以"||"开头，是隐式order-only依赖，仅仅是表示顺序需要的依赖。

# subninja和include，都是用来引入其它的ninja文件。不同点是，subninja引入的子模块可以使用父模块的变量，include引入的子模块不可以使用父模块中的变量。
```

#### 参考文档

- [Ninja文件的基本语法](https://note.qidong.name/2017/08/ninja-syntax/)