是catkin仓库下，bin目录下的一个python脚本。

执行步骤：

1. 在安装catkin或覆盖之前catkin版本的时候，找到相关的可导入的模块。
2. 通过`if __name__ == '__main__'`执行main函数
3. 使用`parse_args()`函数解析传入的参数
4. `apply_platform_specific_defaults()`来自于catkin包的builder模块，用于为win32平台添加特定的参数
5. `handle_cmake_args()`函数，如果定义了参数列表里的cmake参数`XXX_PREFIX`，则转化为其它参数
6. 设置参数列表里的`no_color`参数。
7. 解析参数列表，给相关变量赋值。
8. 使用相关的变量，执行`build_workspace_isolated()`函数。这个函数也来自于`catkin.builder`，它通过cmake和make命令执行构建的过程。

