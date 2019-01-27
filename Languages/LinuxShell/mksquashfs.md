#### 简介

Squashfs是Linux的一个高压缩的只读文件系统。用于archival(打包)或受限的块系统(设备或内存)。

#### 语法

```
mksquashfs SOURCE [...] DESTINATION [OPTIONS]
```

#### 选项

##### 文件系统构建

```
-comp COMPRESSION	# 指定压缩方式COMPRESSION，可用压缩方式有:gzip(默认), lzo, xz
```



##### 文件系统过滤

##### 文件系统追加

```
-noappend	# 不追加到已有文件系统中
```



##### 运行时

##### 实用选项

##### 压缩软件选项