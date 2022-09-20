#### 安装

````
curl -sSf https://sh.rustup.rs | sh

# 使用tuna源加速安装
export RUSTUP_DIST_SERVER=https://mirrors.tuna.edu.cn/rustup
export RUSTUP_UPDATE_ROOT=https://mirrors.tuna.edu.cn/rustup/rustup
curl https://sh.rustup.rs -sSf | sh
````

#### 用法

`rustup [flags] [+toolchain] <subcommand>`

##### 工具链的名称

```
<chanel>[-date][-host]
# <chanel> : stable|beta|nightly
# <date> : yyyy-mm-dd
# <host> : target triple : <cpu-vendor-os>
```



##### flags

```
-v
-q
-h
-V
```

##### subcommand

```
check		# 检查工具链和rustup的更新
completions	# 为shell生成tab补全脚本
component	# 修改工具链的组件
	# rustup component <cmd>
	# <cmd>:
	#	list - 列出已安装的组件和可用的组件
	#	add - 把组件添加到rust工具链
	#	remove - 把组件从rust工具链里删除
	#	help - 打印帮助信息
default		# 设置默认工具链
doc			# 当前工具链的文档
	# rustup doc [flags] [options] [topic]
	# [flags]:
		# --book - 《The Rust Programming Language book》
		# --cargo - 《The Cargo Book》
		# --core - 《The Rust Core Library》
		# --reference - 《The Rust Reference》
		# --std - 《Standard Library》
	# [options]:
		# --toolchain <toolchain> - 指定工具链名称
	# [topic]:
		# 如core, fn, core::arch, std::fs等
help		# 打印帮助信息
install		# 更新工具链
	# rustup install [flags] [options] <toolchain>
	# [flags]:
	#	--force - 强制更新
	# [options]:
	#	--profile <profile> - 可选的值：minimal, default, complete
	# <toolchian>:工具链的名称
man			# 查看给定命令的手册
override	# Modify directory toolchain overrides
run			# 运行一个命令
self		# 修改rustup安装
	# rustup self <cmd>
	# <cmd>:
	# update - 下载并安装对rustup的更新
	# uninstall - 卸载rustup
	# upgrade-data - 更新内部数据格式
	# help
set			# 设置rustup
show		# 显示工具链或设置
	# rustup show <cmd>
	# <cmd>:
	#	active-toolchain - 显示活动的工具链
	#	home - 显示RUSTUP_HOME的值
	#	profile - 显示当前的profile
	#	keys - 显示已有的PGP键
	#	help
target		# 修改工具链的支持对象
	# rustup target <cmd>
	# <cmd>:
	#	list - 列出已安装的对象和可用的对象
	#	add - 为工具链添加对象
	#	remove - 从工具链删除对象
	#	help
toolchain	# 修改或检索已安装的工具链
	# rustup toolchain <cmd>
	# <cmd>:
	# 	list:列出已安装的工具链
	# 	install:安装或更新工具链
	# 	uninstall:卸载工具链
	# 	link:
	# 	help:
update		# 更新工具链和rustup
which		# 显示给定命令的路径
```

#### 示例

```
rustup install nightly
rustup default nightly

rustup target add riscv64gc-unknown-none-elf
rustup component add llvm-tools-preview
rustup component add rust-src
```

#### 错误解决

1 使用rust-objcopy的时候提示`Could not find tool: objcopy`

解决方法：

```
cargo install cargo-binutils
rustup component add llvm-tools-preview
```

2 crates.io的索引无法更新

解决方法：参考cargo的"换源"
