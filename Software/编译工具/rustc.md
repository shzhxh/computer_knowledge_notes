#### 简介

rustc是Rust语言的编译器。

#### 用法

```bash
rustc [options] <input-file>
```

#### 选项

```bash
--emit [asm|llvm-bc|llvm-ir|obj|metadata|link|dep-info|mir]	# 指定编译器的输出
-o <filename>		# 输出到文件filename
-O		# 相当于"-C opt-level=2"
--print <info>	# 打印编译器的信息。可用的info如下：
	# target-list : 显示CPU架构，厂商，操作系统的信息。
-V, --version	# 打印版本信息。
-v, --verbose	# 打印详细信息。

# 更多的帮助
-C help		# 代码生成
-W help		# lint选项和默认设置
-Z help		# 不稳定的编译器选项
--help -v	# 所有的选项
```

#### 代码生成选项