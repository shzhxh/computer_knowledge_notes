#### 简介

子模块就是将一个仓库作为另一个仓库的子目录

```
git submodule <command> [options]	# 初始化，更新或分析子模块
```

#### 命令

##### add

```
add [options] [--] <repo> [path]	# 添加子模块。
# -b <branch>
# -f, --force
# --name <name>
# --reference <repo>
# --depth <depth>
```

##### status

显示子模块的状态。

##### init

依据`.git/config`初始化子模块。

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

显示commit汇总。

##### foreach

在每个子模块里执行指定的命令。

##### sync

同步子模块。

##### absorbgitdirs

#### 选项

#### 示例

可以查看.gitmodules文件查看文件夹与子模块的对应关系

```shell
# 添加子模块
git submodule add <repo> [path]

# 克隆包含子模块的项目，默认包含子模块的目录，但目录内没有任何文件,需要如下命令使子模块充实起来
git submodule init	# 初始化子模块
git submodule update	# 更新子模块
	# --init --recursive = "git submodule init" + "git submodule update --recursive"

# 删除子模块
git submodule deinit [path]
git rm --cached [path]
vim .gitmodules	# 删除[path]对应的条目
rm -r .git/modules/[path]	# 删除脏文件
```

#### 错误解决

1. could not get a repository handle for submodule 'rboot'

   ```
   # git submodule update出错此错误提示，删除了rboot目录下的.git文件即可，原因未知。
   # 曾尝试过git submodule deinit，无效
   # 曾尝试过rm -rf .git/modules/rboot，无效
   ```

2. 子模块会自己checkout到某个提交点

   ```
   # 我希望子模块能自己随着远程仓库的更新而自己更新，但实际上不会的。通过观察执行了git submodule add的那个分支，可以发现子模块的commit id也被记录下了。所以，让子模块更新的方法是：
   cd <submodule> && git pull
   cd .. && git add <submodule> && git commit -m ""
   ```

3. fatal: No url found for submodule path 'xxx' in .gitmodules

   ```
   # 可能是仓库的原主人需要子模块xxx,但我不需要它，我只要把它删除即可
   git rm --cached xxx
   ```

   
