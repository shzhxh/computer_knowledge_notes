#### 简介

cargo是rust的包管理器。

#### 用法

```bash
cargo [options] <command> [args]
cargo [options] --version
cargo [options] --list
cargo [options] --help
cargo [options] --explain <CODE>
```

#### 命令

```bash
# 构建命令
build	# 编译一个包。
	# 不使用"--release"时采用dev配置
	# 使用"--release"时采用release配置
	# 可以在Cargo.toml中使用[profile.*]覆盖任意设置的默认值。
doc		# 构建一个包的文档。生成的是HTML格式。
	# 使用"///"来注解格式化文档，以支持Markdown
	# 使用"//!"用于描述整个crate。
	# 使用"pub use"来把类型重导出到顶级结构，以便用户查询。
	## 文档选项
	--open	# 先构建文档，再在浏览器中打开它。
	## 包选项
	## 目标选项
	## 属性选项
	## 编译选项
	## 输出选项
	## 显示选项
	## Manifest选项
	## 通用选项
	## 其它选项
run		# 编译并运行本地包
	## 包选项
	## 目标选项
	## 属性选项
	## 编译选项
	--target <TRIPLE>	# 在指定架构上运行。也可以在配置文件里通过"buld.target"的值来指定。
	## 输出选项
	## 显示选项
	## Manifest选项
	## 通用选项
	## 其它选项
test	# 对包执行单元和集成测试。用`--`分隔，传给cargo的放前面，传给待测二进制文件的放后面。
	## 传给cargo的参数
	[TESTNAME]	# 只运行名字里包含TESTNAME的测试
	-j,--jobs <N>	# 构建二进制的作业数量。此参数不影响运行测试的线程数。
	--test <NAME>	# 只测试tests目录下文件名为NAME.rs的代码
	--tests			# 测试tests目录下所有的文件
	## 传给二进制文件的参数
	--test-threads=<N>	# 指定运行测试的线程数量。
	--nocapture	# 测试成功时默认截获输出的内容，使用此选项可禁用截获输出的行为。
	--ignored	# 在代码里加`#[ignore]`可忽略指定的测试，使用此选项则会运行被忽略的测试。
	--include-ignored	# 即运行忽略的，也运行不被忽略的测试
# manifest命令

# 包命令
install	# 构建和安装一个rust库。
new	# 创建一个cargo包
	--bin	# 默认选项。包里会有src/main.rs(二进制目标)。
	--lib	# 包里会有src/lib.rs(库目标)。

# publishing命令
login	# 把注册中心的令牌保存到本地。
publish	# 把包上传到注册中心。
yank	# 从索引中删除一个已上传的crate。

# 通用命令
help
version
```

#### 选项

```bash
# 特定选项
--list	# 列出所有已安装的子命令。
# 显示选项

# manifest选项

# 普通选项
```

#### 相关文件

- Cargo.toml

  如以[workspace]开始，则用于设置工作空间。通过给"members"赋值增加成员。

