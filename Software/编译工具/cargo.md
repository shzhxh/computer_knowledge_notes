#### 简介

cargo是rust的包管理器。

#### 用法

`cargo [options] <command> [args]`

#### 命令

```bash
# 构建命令
run		# 编译并运行本地包
test	# 对包执行单元和集成测试。用`--`分隔，传给cargo的放前面，传给待测二进制文件的放后面。
	# 传给cargo的参数
	[TESTNAME]	# 只运行名字里包含TESTNAME的测试
	-j,--jobs <N>	# 构建二进制的作业数量。此参数不影响运行测试的线程数。
	# 传给二进制文件的参数
	--test-threads=<N>	# 指定运行测试的线程数量。
	--nocapture	# 测试成功时默认截获输出的内容，使用此选项可禁用截获输出的行为。
	--ignored	# 在代码里加`#[ignore]`可忽略指定的测试，使用此选项则会运行被忽略的测试。
	--include-ignored	# 即运行忽略的，也运行不被忽略的测试
# manifest命令

# 包命令
new	# 创建一个cargo包
	--bin	# 默认选项。包里会有src/main.rs(二进制目标)。
	--lib	# 包里会有src/lib.rs(库目标)。

# publishing命令

# 通用命令
help
version
```

#### 选项

```bash
# 特定选项

# 显示选项

# manifest选项

# 普通选项
```

#### 相关文件