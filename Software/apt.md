#### 简介

包管理系统的命令行接口。它被设计为仅面向普通用户，所以仅包含了最常用的命令和选项。

#### 子命令

- list
- search
- show
- update
- install
- remove：
- autoremove：曾经因为依赖关系而自动安装的包，现在已不再需要，可用此命令卸载。
- autoclean：如果一个软件包已被卸载，则删除它的安装文件。
- clean：删除所有安装文件。
- upgrade
- edit-sources：编辑`sources.list`

#### 用法示例

```
sudo apt install ubuntu-desktop		# install unity
sudo apt install xxxx:i386			# 安装软件包并指定其架构为i386
```