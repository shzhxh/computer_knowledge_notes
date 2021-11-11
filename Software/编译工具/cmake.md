#### 简介

`cmake`的核心功能就是生成一个构建系统。构建系统，描述了如何使用构建工具自动从源码构建可执行文件和库。比如，Makefile就是一个构建系统，它的构建工具就是`make`命令。`cmake`使用CMake语言，通过一个叫生成器的后端，来生成这个构建系统。

要生成一个构建系统，必须要指定的是：源码树、构建树、生成器。源码树在`CMakeLists.txt`里指定，构建树在`CmakeCache.txt`里指定，生成器通过`-G`选项指定，可以通过`cmake --help`查看当前都支持哪些生成器。

#### 用法

```bash
# 生成一个项目的构建系统
cmake [options] <path-to-source>
cmake [options] <path-to-existing-build>
cmake [options] -S <path-to-source> -B <path-to-build>
# 构建一个项目
cmake --build <dir> [<options>] [-- <build-tool-options>] 
# 安装一个项目
cmake --install <dir> [<options>]
# 打开一个项目
cmake --open <dir> 
# 运行一个脚本
cmake [{-D <var>=<value>}...] -P <cmake-script-file>
# 运行命令行工具cmd
cmake -E <cmd> [options]	
# 运行find-package工具
cmake --find-package [options]
# 帮助信息
cmake --help [-topic]
```

#### 选项

##### 生成构建系统

```bash
-S <path-to-source>	# 指定源码目录
-B <path-to-build>	# 指定构建目录
-G <generator-name>	# 指定生成器

-E	# cmake命令模式
```

##### 构建项目

```
```



##### 安装项目

```
```



##### 运行命令行工具

```
```



##### 帮助信息

```
--help	# 打印帮助信息
--help-command <cmd>	# 打印命令<cmd>的帮助信息。
```



#### 参考信息

```
man cmake
man 7 cmake-language
man 7 cmake-buildsystem
man 7 cmake-generators
https://cmake.org
```

