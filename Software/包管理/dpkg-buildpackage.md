#### 简介

用以生成debian包的二进制工具。

#### 用法

```
dpkg-buildpackage [option]		# 生成debian包
```

#### 选项

```
-build=type		# 定义构建类型type，type可以取如下的值：
		# source	: 构建源包。
		# any		: 构建架构相关的二进制包
		# all		: 构建架构无关的二进制包
		# binary	: 构建二进制包，相当于 any,all
		# full		: 默认选项，编译所有的东西，相当于 source,any,all
		
-b		# 等价于-build=binary，意思是构建二进制包 
-uc, --unsigned-changes		# 不对.buildinfo和.changes签名
-us, --unsigned-source		# 不对源包签名
```

