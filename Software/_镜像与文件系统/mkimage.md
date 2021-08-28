#### 简介

为U-Boot生成镜像。这些镜像可以包含linux内核、设备树文件、根文件系统镜像、固件镜像等，即可以分开也可以合在一起。`mkimage`支持两种格式，旧的传统格式和新的FIT(Flattened Image Tree)格式。

#### 语法

```
mkimage -l [image-name]
mkimage [options] -f [sorce-file] [image-name]
mkimage [options] -F [image-name]
mkimage [options] [image-name]		# 创建传统格式的镜像
```

#### 选项

##### 列出镜像信息

```
-l [image-name]	# 
```



##### 创建传统镜像

```
-a [load-addr]	# 设置加载地址，此地址为16进制数。
-A [arch]	# 设置架构为[arch]。
-C [compression-type]	# 设置压缩类型。
-d [image-data]		# 镜像的数据来自于[image-data]。
-e [entry-point]	# 设置入口点，此地址为16进制数。
-n [image-name]	# 设置镜像名。
-O [os]		# 设置操作系统为[os]。
-T [image-type]	# 设置镜像类型。
```



##### 创建FIT镜像