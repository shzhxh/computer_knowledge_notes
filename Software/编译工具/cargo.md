### 参考资料

- [cargo](https://doc.rust-lang.org/cargo/commands/cargo.html)
- `rustup doc --cargo`
- `cargo help <cmd>`

### 简介

cargo是rust的包管理器。

```bash
cargo [options] <command> [args]
cargo [options] --version
cargo [options] --list
cargo [options] --help
cargo [options] --explain <CODE>
```

### 命令

#### 构建命令

```bash
bench [options] [benchname][-- bench-options]	# 编译并运行benchmark。
run	[options] [-- args]	# 编译并运行本地包
	## 包选项
	-p, --package <spec>	# 指定要运行的包。关于<spec>的格式见cargo-pkgid。
	## 目标选项
	## 属性选项
	## 编译选项
	--target <TRIPLE>	# 在指定架构上运行。也可以在配置文件里通过"buld.target"的值来指定。
	--release	# 使用release属性进行编译优化。
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
```

##### build

```bash
build [options]	# 编译一个包。
	# 注：不使用"--release"时采用dev配置
	# 注：使用"--release"时采用release配置
	# 注：可以在Cargo.toml中使用[profile.*]覆盖任意设置的默认值。
## 包选项
  -p <specs>
  --workspace
  --all
  --exclude <SPECS>
## 目标选项
  --lib
  --bin <names>
  --bins
  --example <names>
  --examples
  --test <names>
  --tests
  --bench <names>
  --benches
  --all-targets
## 属性选项
  -F <features>
  --all-features
  --no-default-features
## 编译选项
	--traget <triple>	# 指定编译的架构(默认是主机的架构)。triple的格式为<arch><sub>-<vendor>-<sys>-<abi>。
		# 也可以在配置文件里通过设置build.target实现同样的功能。
		# 注：编译结果会放在不同的目录中。
	-r, --release	# 使用release配置来编译要优化的组件。
	--profile <name>
	--ignore-rust-version
	--timings=<fmts>
## 输出选项
  --target-dir <dir>
  --out-dir <dir>
## 显示选项
  -v, --verbose	# 输出详细信息。两次指定此参数可输出更详细的信息。
  -q
  --color <when>
  --message-format <fmt>
  --build-plan
## Manifest选项
	--manifest-path <path>	# 指定Cargo.toml的路径。默认是从当前路径或任意父路径中查找。
	--frozen, --locked
	--offline
## 通用选项
	+<toolchain>	# 指定工具链的名称。（如+stable，+nightly）。前提要求：cargo是通过rustup安装的，此参数为cargo的第一个参数。
	--config <KEY=VALUE>
	-h
	-Z <flag>	# Unstable flags to Cargo。(仅用于nightly)
## 其它选项
	-j, --jobs <N>
	--keep-going
	--future-incompat-report
```

##### check

```bash
check [options]	# 检查当前的包
## 包选项
## 目标选项
## 属性选项
	-- all-features	# 激活所有可用的属性
## 编译选项
## 输出选项
## 显示选项
## Manifest选项
## 通用选项
## 其它选项
```

##### doc

```bash
doc	[options]	# 构建一个包的文档。生成的是HTML格式。
	# 使用"///"来注解格式化文档，以支持Markdown
	# 使用"//!"用于描述整个crate。
	# 使用"pub use"来把类型重导出到顶级结构，以便用户查询。
	## 文档选项
	--open	# 先构建文档，再在浏览器中打开它。
	--no-deps	# 不为依赖项构建文档。
	--document-private-items	# 在文档中包含非公开的项。
	## 包选项
	--all
	--exclude
	-p
	--workspace
	## 目标选项
	--bin
	--bins
	--example
	--examples
	--lib
	## 属性选项
	--all-features	# 激活所有属性。
	--features <features>	# 指定要激活的属性。
	--no-default-features	# 不激活default属性。
	## 编译选项
	--ignore-rust-version
	--target
	--profile
	-r
	## 输出选项
	--target-dir
	## 显示选项
	-v
	-q
	--color
	--message-format
	## Manifest选项
	--manifest-path
	--frozen, --locked
	--offline
	## 通用选项
	+<toolchain>	#
	-h, --help		#
	-Z <flag>		#
	## 其它选项
	-j, --jobs <N>	# <N>个作业并行执行
```



#### manifest命令

```
update [options]	# 把Cargo.lock里的依赖更新到最新的版本。Cargo.lock如不存在则创建。
```



#### 包命令

##### search

```
search [options] [query...]	# 从crates.io查找包
```

##### new

```
new [options] <path>	# 创建一个cargo包
	--bin	# 默认选项。包里会有src/main.rs(二进制目标)。
	--lib	# 包里会有src/lib.rs(库目标)。
```

##### init

```
init [options] [path]	# 基于已有目录创建Cargo包
	# 初始化选项
	--bin	# 创建带有二进制目标(src/main.rs)的包。这是默认行为。
	--lib	# 创建带有库目标(src/lib.rs)的包。
	--edition <edition>	# 定义要使用的Rust版本。默认2021。
	--name <name>	# 设置包名。默认为目录名。
	--vcs <vcs>	#
	--registry <reg>	#
	# 显示选项
	-v
	-q
	--color <when>
	# 通用选项
	+toolchain
	--config <KEY=VALUE>
	-h
	-Z <flag>
```

##### install

构建和安装一个rust二进制文件。只有包含**bin**和**example**对象的包才能被安装，所有的可执行文件都被安装在`bin`目录。

```
install [options] <crate>[@version]	# 从crate.io安装
install [options] --path <path>		# 从本地路径安装
install [options] --git <url> [crate]	# 从git仓库安装
install [options] --list
	# 安装选项
	--git <url>	# 从<url>来安装指定的库。
	--rev <sha>	# 从git仓库安装的时候指定commit号
	--list		# 列出所有已安装的包和它们的版本号
	-f, --force	# 如果库或二进制文件已存在，则覆盖它们。
	# feature选项
	# 编译选项
	# 清单选项
	# 其它选项
	# 显示选项
	# 普通选项
```



#### publishing命令

```
login	# 把注册中心的令牌保存到本地。
publish	# 把包上传到注册中心。
yank	# 从索引中删除一个已上传的crate。
```



#### 通用命令

```
help
version
```

#### 其它命令

不是cargo自带，需要用户自己安装的命令。

```bash
cargo clippy [options] [--] [opts]
# 作用：检查package里的常见错误，提升rust代码的质量
	# 选项（其它选项同rust check）
	--no-deps
	--fix
	# opts
	-W --warn OPT
	-A --allow OPT
	-D --deny OPT
	-F --forbid OPT
```



### 选项

```bash
# 特定选项
--list	# 列出所有已安装的子命令。
# 显示选项

# manifest选项

# 普通选项
```

### 配置

cargo的配置文件是`.cargo/config`或`.cargo/config.toml`(当两者都存在时，cargo会读取没有后缀的那个，但是推荐带上后缀)，它会在当前目录和所有父目录中查找，最后在`~`查找。对于数字、字符串和布尔值，靠前的配置会覆盖靠后的配置；对于数组，所有的配置会连接起来；一般使用家目录放默认配置，这样就可以实现对每个包的精确配置。

```
# [build]
target
	# 指定目标三元组<cpu-vendor-os>，例如"riscv64gc-unknown-none-elf"

# [target.<triple>]	如[target.riscv64gc-unknown-none-elf]
linker = "..."		# 指定链接器
runner = "..."		# 指定wrapper，用来运行可执行文件
rustflags = ["...", "..."]	# 指定rustc的编译参数

# [net]用于配置网络
git-fetch-with-cli
	# 布尔型，默认false。如为true，则使用系统自带的git命令。如为false，则使用内建的git库。
```



### 相关文件

##### Cargo.toml

- [workspace]，用于设置工作空间(workspace)。工作空间是多个包的集合，这些包就叫做工作空间的成员(workspace members)。有两种工作空间：作为root package，或作为虚拟清单(virtual manifest)。

  `Cargo.toml`里即有`[workspace]`段也有`[package]`段，这种情况下这个package就是root package。工作空间的根目录(workspace root)就是工作空间的`Cargo.toml`所在的目录。

  `Cargo.toml`里只有`[workspace]`段没有`[package]`段，这就叫虚拟清单。如果没有"主"包，或想让所有的包都在不同的目录下，则使用虚拟清单。

  工作空间的要点是：

  - 所有的包共用工作空间根目录下的`Carogo.lock`文件。
  - 所有的包共用同一个输出目录，这个输出目录默认是工作空间根目录下的`target`目录。
  - `Cargo.toml`里的`[patch], [replace], [pofile.*]`段只在根清单里被识别，而成员清单里则被忽略。

  注意：工作空间里的所有存在依赖关系的路径，会自动成为工作空间的成员。所以`[package]`字段带着一个空的`[workspace]`字段，就是创建了一个包含这个包和它所有依赖路径的工作空间。

  ```
  # [workspace]段
  members		# 定义工作空间里的成员，是由目录字符串组成的数组，支持通配符。
  exclude		# 避免把路径包含进工作区
  default-members	# 在工作空间的根目录下，命令行里没有使用-p或--workspace标志显式地指定要操作的包，则使用此选项里定义的包作为要操作的包。
  
  # 选择工作空间
    # 当进到工作空间的子目录，cargo会自动从父目录里找有[workspace]定义的Carogo.toml文件，以决定使用哪个工作空间。但也可以在成员库里使用 package.workspace 来手动指定工作空间的根目录。
    
  # 选择包
    # 在工作空间里，与包相关的cargo命令(比如cargo build)可以用-p 或--workspace来指定要操作的包。如以上两个选项都没使用，cargo会使用当前工作目录下的包。如果当前目录是虚拟工作空间，则会添加所有的成员(和在命令行里给出--workspace的效果是一样的)。
    
  # [workspace.metadata]
    # cargo会忽略它且不会报警。那些在Cargo.toml里保存工作空间配置的工具会用到它。虽然cargo没有为它的内容指定格式，但建议外部工具以统一的方式来使用它。例如，如果[package.metadata]缺数据的话则引用[workspace.metadata]里的数据。
  ```

- 如以[package]开始，则用于定义package。

  ```
  name		# 包名
  version		# 包的版本号
  authors		# 包的作者
  edition		# Rust版本
  ```

- [features]，条件编译的属性

  ```
  bmp = []	# 定义名叫bmp的属性，它不会使能其它属性
  png = []
  ico = ["bmp", "png"]	# 如使能了ico，则会使能bmp、png
  ```

  

- [profile.release]，用于release的优化

  ```
  lto	# 通过-C lto来控制LLVM的链接时间优化。false代表瘦LTO，true代表富LTO，thin介于瘦和富之间，off则不用LTO。
  ```

  

- [dependencies]，对库的依赖

  ```
  1.2.3 := >=1.2.3, <2.0.0	# 最大依赖.即允许非０的最高序号加１.
  ^1.2.3 := >=1.2.3, <2.0.0	# 等价于最大依赖，
  ~1.2.3 := >=1.2.0, <1.3.0	# 最小依赖，即允许非０的最低序号内变化
  1.2.* := >=1.2.0, <1.3.0	# 通配符依赖，即指定序号内变化
  >= 1.2.0
  > 1
  < 2
  = 1.2.3			# 精确指定版本号为1.2.3
  >= 1.2, < 1.5	# 多重依赖使用逗号来分隔
  # 非crates.io的包，要使用registry关键字
  # git仓库使用git关键字，可附带rev、tag、branch键指定提交号、标签、分支
  # 路径依赖使用path关键字
  ```

  

- [target]，特定平台的依赖

  ```
  [target.'cfg(target_arch="riscv64")'.dependencies]	# 可以使用#[cfg]语法
  ```

  

### 换源

```
修改~/.cargo/config
[source.crates-io]
replace-with = 'tuna'

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```

### 错误记录

1. ````
   can't find crate for `std`
   note: the `riscv64gc-unknown-none-elf` target may not be installed
   ````

   > 使用rustup target list命令可以看见riscv64gc-unknown-none-elf已经安装了。我之前在~/.cargo/config里新建了一个条目build.target="riscv64gc-unknown-none-elf"，当我把这个条目删除后就不再有这个错误提示了。错误原因未知。

2. ```
   failed to load source for dependency `bitmap-allocator`
   ```

   > cargo使用了自带的git命令，导致git命令执行失败。参考“配置”相关章节进行设置。

