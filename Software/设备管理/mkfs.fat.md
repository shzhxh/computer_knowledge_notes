#### 简介

在Linux下创建MS-DOS文件系统

```
mkfs.fat [options] <dev> [block-count]
	# <dev> : 设备文件或镜像文件。当使用-C选项时镜像文件可以不存在。
	# [block-count] : 设备上块的数量。每个块固定为1024字节，与扇区大小或簇(cluster)大小无关。当使用-C选项后必须指定此参数。
```



#### 选项

```bash
-C	# 创建<dev>对应的镜像文件，这样就不需要再用dd命令来创建文件了。使用此选项后必须指定[block-count]。
-F <FAT-SIZE>	# 指定文件分配表的类型(12, 16或32位)。
-n <volume-name>	# 指定卷的名称。
```

