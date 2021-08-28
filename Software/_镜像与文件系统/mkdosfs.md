#### 简介

`mkdosfs`只是`mkfs.fat`的一个符号链接。它的作用是在Linux下创建一个MS-DOS文件系统。

#### 语法

```
mkfs.fat [OPTIONS] DEVICE [BLOCK-COUNT]	# 在DEVICE上创建MS-DOS分区。BLOCK-COUNT指定设备上块的个数。
```

#### 选项

```
-c	# 在创建文件系统前检查设备里坏掉的块。
-C	# 创建一个文件作为DEVICE，并写入要创建的文件系统。当使用此选项时，必须给定BLOCK-COUNT的值。
-n	VOLUME-NAME	# 设置文件系统的卷名(标签label)。
```

