```
git submodule <command> [options]	# 初始化，更新或分析子模块
```

#### 命令

##### add

##### status

##### init

##### deinit

```
deinit [-f] <--all | [--] path...>	# 删除本地子模块
```



##### update

```
update [options] [--] [<path>...] 
# 更新已注册的模块，使用的方法是克隆子模块并更新子模块的工作树。
# 如果子模块还没有初始化，并且只是使用.gitmodules里的设置，可使用--init选项自动初始化子模块。
# --recursive的作用是，递归地更新子模块里的子模块。
```



##### summary

##### foreach

##### sync

##### absorbgitdirs

#### 选项

#### 示例

可以查看.gitmodules文件查看文件夹与子模块的对应关系

```shell
# 子模块就是将一个仓库作为另一个仓库的子目录
git submodule add <repo> <path> # 添加子模块

# 克隆包含子模块的项目，默认包含子模块的目录，但目录内没有任何文件,需要如下命令使子模块充实起来
git submodule init	# 初始化子模块
git submodule update	# 更新子模块
	# --init --recursive = "git submodule init" + "git submodule update --recursive"

```

#### 错误解决

1. could not get a repository handle for submodule 'rboot'

   ```
   # git submodule update出错此错误提示，删除了rboot目录下的.git文件即可，原因未知。
   # 曾尝试过git submodule deinit，无效
   # 曾尝试过rm -rf .git/modules/rboot，无效
   ```

   
