#### 子模块

可以查看.gitmodules文件查看文件夹与子模块的对应关系

```shell
# 子模块就是将一个仓库作为另一个仓库的子目录
git submodule add <repo> <path> # 添加子模块

# 克隆包含子模块的项目，默认包含子模块的目录，但目录内没有任何文件,需要如下命令使子模块充实起来
git submodule init	# 初始化子模块
git submodule update	# 更新子模块
	# --init --recursive = "git submodule init" + "git submodule update --recursive"

```

