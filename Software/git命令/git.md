#### 参考资料

- [git内部原理](https://www.open-open.com/lib/view/open1328070620202.html#articleHeader3)
- [git底层数据结构和原理](https://yq.aliyun.com/articles/761686)

#### 重要概念

- 分支：每次提交都是一个点，相邻的两次提交连成线，多条线连成一条路径，每条路径就是一个分支。
- HEAD：当前工作区所在的位置，这个位置可以是任意的某个提交。
- 工作区(workspace)：仓库所在的目录，包括除.git目录外的所有文件和目录。
- 暂存区(index)：使用`git add` 命令把工作区的内容放到暂存区，暂存区在.git目录里。
- 本地仓库(local repository)：使用`git commit`命令把暂存区的内容放到本地仓库，本地仓库也在.git目录里。
- 远程仓库(remote repository)：使用`git push`命令把本地仓库里的内容放在远程仓库。
- blob：在git内存储的不是文件名而仅仅是文件的内容，这种对象被称为blob。
- tree：在tree里存储的是blob或其它tree，类似于数据结构里树的非叶子结点，这种对象被称为tree。
- commit：保存了tree的创建者、创建时间和注释的信息，这种对象被称为commit。
- references：也叫refs或引用，是commit ID的别名，方便记忆。
- tag：对commit的引用，有两种tag：annotated和lightweight。

#### 语法

```
git [optons] <command> [args]
```

#### 选项

```

```

#### 高层命令(porcelain)

##### 主要命令

```
git-add 	# 把工作区的文件加到暂存区
git-am  	# 打补丁
git-archive # 把文件归档
git-bisect	# 使用二分法查找产生bug的提交
git-branch	# 分支管理
git-bundle	# 通过归档移动对象和引用
git-checkout	# 切换分支
git-cherry-pick	# 对已存在的commit进行再提交
git-citool	#
git-clean	# 删除工作目录中未追踪的文件
git-clone	# 复制仓库
git-commit	# 向仓库中提交
git-describe	# 
git-diff	# 打印各种差别
git-fetch	# 从其它仓库下载对象和引用
git-format-patch
git-gc		# 清除不必要的文件并优化本地仓库
git-grep
git-gui
git-init	# 初始化仓库
git-log		# 打印提交的日志
git-merge	# 合并多个开发历史
git-mv		# 移动文件
git-notes	# 
git-pull	# fetch并合并远程仓库(或本地分支)
git-push	# 更新远程仓库
git-rebase
git-reset	# 将HEAD重置到指定的状态
git-revert	# 重置一些已知提交
git-rm		# 从工作树或暂存区中删除文件
git-shortlog
git-show	# 显示对象的各种类型
git-stash
git-status	# 显示工作树的状态
git-submodule	# 子模块管理
git-tag		# 标签管理
git-worktree
gitk
```

##### 辅助命令

```
git-config	# 设置git
git-fast-export
git-fast-import
git-filter-branch
git-mergetool
git-pack-refs
git-prune
git-reflog	# 管理reflog信息
git-remote	# 管理对远程仓库的追踪
git-repack
git-replace
... ...
```

##### 与外界交互

与其它版本管理软件交互或与其他人通过email交互

#### 底层命令(plumbing)

##### 操作命令

```
git-apply
git-checkout-index
git-commit-tree		# 创建一个commit对象
 -p <parent>	# 用于将两个commit关联起来，建立先后次序
git-hash-object		# 计算object ID，且可选择地创建一个blob
　-w		# 指示要存储一个对象，若无此参数则仅返回一个键值
 -stdin	# 指示从标准输入读取内容，否则需指定要存储的文件路径
git-index-pack
git-merge-file
git-mktag
git-mktree
git-pack-objexts
git-prune-packed
git-read-tree		# 将tree的信息读入暂存区
git-symbolic-ref	# 读取、修改和删除符号引用
git-unpack-objects
git-update-index	# 将文件内容从工作区注册到暂存区
git-update-ref		# 更新存储在ref里的对象名
git-write-tree		# 从缓存区创建一个tree对象
```

##### 询问命令

```
git-cat-file	# 从仓库objects里获取内容、类型或大小的信息
	-t	# 显示object的类型
	-s	# 显示object的大小
	-p	# 打印出object的内容
... ...
```

##### 同步仓库

```
git-daemon
... ...
```

##### 内部帮助命令

由其它命令来使用的内部帮助命令，最终用户一般不会直接使用它们。

```
git check-attr	# 
... ...
```

#### 获取帮助

```shell
man git	# git详解
man gittutorial	# git初学者指引
man gitevery	# git实用命令集
man gitcli	# git命令概述
git help <commond>	# 单个命令commond的信息
```

#### 内部原理

从根本上来说，git是一套内容寻址(content-addressable)文件系统，在此之上附加了一个版本控制系统的用户接口。

git的用户接口分两层：

- 高层命令：也叫porcelain，它是用户友好的，一般从命令行手工执行。
- 底层命令：也叫plumbing，它是用户不友好的，一般用来为其它工具和脚本服务。

`.git`目录包含了git存储和操作的所有内容，是理解git原理的核心。其结构如下：

| 目录或文件     | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| COMMIT_EDITMSG | 文本文件，最后一次commit的注释                               |
| config         | 项目特有的配置选项。                                         |
| description    | 仅供gitweb程序使用。                                         |
| FETCH_HEAD     | 文本文件，本地仓库的每个分支最后一次和远程仓库交互时的commit id |
| **HEAD**       | 文本文件，其内容是当前工作区所在的分支，是git的核心部分。    |
| **index**      | 保存了暂存区的信息，可以用`git ls-files`命令查看其内容，是git的核心部分。 |
| ORIG_HEAD      | 文本文件，在进行风险操作(reset,merge,rebase)时，保存HEAD指向的commit id |
| packed-refs    | 如执行了`git gc`命令，refs目录中将会删除某些文件，为保证效率，这些被删除的文件的内容将会保存到packed-refs文件里 |
| branchs/       | 目录，新版本的git不再使用此目录。                            |
| hooks/         | 目录，客户端或服务器端勾子脚本。                             |
| info/          | 保存了一份不希望在`.gitignore`文件中管理的忽略模式 (ignored patterns) 的全局可执行文件。 |
| logs/          | 保存了各个分支的提交日志                                     |
| **objects/**   | 存储所有的数据内容，是git的核心部分。                        |
| **refs/**      | 存储指向数据的提交对象的指针，是git的核心部分。              |

内容寻址文件系统是一个key-value数据存储系统。

插入的任意内容都对应一个40位的哈希值，通过这个哈希值可以方便地取出所对应的内容。此哈希值的前2位将作为objects目录下的子目录名，后38位将作为子目录下的文件名。这种只记录内容而不存储文件名的对象称为**blob对象**。哈希值是通过文件头和文件内容拼接在一起计算出来的，文件头的格式为`对象类型+数据长度+空字节`。

blob对象是通过**tree对象**组织在一起的。一个tree对象由一条或多条记录组成，每条记录都是一个blob对象或其它tree对象。

一个**commit对象**则是对一个tree对象的描述，它的格式是比较简单的。

一个**reference**是对一个commit取了个别名，方便记忆。HEAD文件保存了当前分支的reference，这样在分支切换的时候就可以指向最后一次提交的commit ID。

一个**tag对象**是对一个commit的引用。共有两种tag：lightweight和annotated。lightweight tag仅仅是对一个commit的引用。annotated tag则会创建一个tag对象，此tag对象指向commit。