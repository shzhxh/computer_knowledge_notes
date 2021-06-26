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
default		# 设置默认工具链
doc			# 当前工具链的文档
help		# 打印帮助信息
man			# 查看给定命令的手册
override	# Modify directory toolchain overrides
run			# 运行一个命令
self		# 修改rustup安装
set			# 设置rustup
show		# 显示工具链或设置
target		# 修改工具链的支持对象
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

