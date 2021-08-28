#### 简介

ndk-build是一个构建脚本，它会使用Android.md和Application.md配置文件。

#### 选项

```
clean	# 删除之前生成的二进制
V=1		# 打印出构建过程的详细命令
-B		# 强制彻底地重新构建
NDK_LOG=1	# 显示NDK内部日志，用于调试NDK
NDK_DEBUG=1	# 强制可调试地构建
NDK_BEBUG=0	# 强制release build
NDK_HOST_32BIT=1	# 总是使用32位模式的工具链(默认使用64位工具链)
NDK_APPLICATION_MK=<file>	# 使用文件file构建
-C <project>	# 在指定目录project构建本地代码
```

#### Android.mk

##### 简介

`Android.mk`在`jni`目录里，描述了构建系统的sources和共享库。

##### 变量与宏

##### 模块描述变量

#### Application.mk

##### 简介

`Application.mk`在jni目录里，定义了一些编译用的变量。

##### 变量

