#### 解决的问题

#### 内部的结构

##### lkp脚本

lkp脚本的作用就是分发子命令，即在指定目录下，为子命令找到其对应的命令。

1. 首先要定义变量LKP_SRC
2. 定义show_help函数，即打印一些字符串
3. 如没有参数或参数为help，则执行show_help函数并退出
4. 如第一个参数为help，则第二个参数为真正的命令，将打印真正的命令的帮助信息。
5. 第一个参数如果不是help，则是一个要执行的命令，为其指定真正对应的命令。
6. 定义try_run函数，即以第二参数为子目录，其余参数为参数，执行真正的命令。
7. 执行try_run函数，尝试以bin, sbin, tools, lkp-exec为第一个参数执行之。
8. 如以上执行try_run函数失败，则打印执行失败信息并退出。

##### 打包

运行`lkp pack <bm_name>`执行的一系列操作，包括下载、编译、安装、打包。由于lkp命令的执行是分别从bin, sbin,tools, lkp-exec分别查找对应命令尝试执行，而仅在sbin目录下有pack命令，所以`lkp pack`真正执行的是`sbin/pack`命令。

1. 首先要定义变量LKP_SRC

2. usage函数介绍了pack命令的用法

   ```
   pack [options] <BM_NAME>
   	# options
   	-a <arch>
   	-c
   	-d <distro>
   	-s <dest>
   	-f
   ```

3. 用getopts命令在while循环里解析传过来的参数

4. 通过source命令引入一些函数，由于自定义的脚本是最后引入的，所以自定义脚本可以覆盖前面的脚本。

5. download函数完成下载

6. build函数完成编译

7. install函数完成安装

8. pack_pkg函数完成打包

##### 本地运行

##### 虚拟机运行

#### 使用方法

##### 打包脚本

##### 配置脚本

##### 测试脚本

##### 数据脚本