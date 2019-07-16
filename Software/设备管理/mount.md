#### 语法

```bash
mount [-l|-h|-V]
mount -a [-fFnrsvw][-t fstype][-O optlist]	# 挂载fsbab里记录的所有文件系统
mount [-fnrsvw][-o options] device|dir
mount [-fnrsvw][-t fstype] [-o options] device dir
```

#### 选项

```
-a, --all	# 挂载fsbab里记录的所有文件系统
-B
-c
-F
-f
-i
-L
-l	# 显示已加载的文件系统
-M
-n
-o, --options <list>	# 指定挂载选项list，list内部的各选项用逗号分隔，可用的选项详见#文件系统无关选项和#文件系统相关选项
-O, --test-opts <list>	# 与-a一起使用，限制文件系统的集合
-R
-r
-s
--source
--target
-T
-t	# 指示文件系统的类型
-U	
-v	# 详细模式
-w
-V	# 打印版本信息
-h	# 打印帮助信息
```

#### 文件系统无关选项

```
remount	# 重新挂载文件系统。它一般用于修改挂载标志，尤其是让只读的文件系统改为可写。
ro		# 以只读的形式挂载文件系统
rw		# 以可读写的形式挂载文件系统
```



#### 文件系统相关选项

##### adfs

##### affs

##### btrfs

cifs

cohernet

debugfs

devpts

ext2,ext3,ext4

fat

hfs

hpfs

iso9660

jfs

minix

msdos

ncpfs

nfs,nfs4

ntfs

overlay

proc

ramfs

reiserfs

romfs

squashfs

smbfs

sysv

tmpfs

ubifs

udf

ufs

umsdos

vfat

usbfs

xenix

xfs

#### 示例

```bash
mount -t type device dir	# mount的标准形式，将设备device上type类型的文件系统挂载到目录dir上
mount /tmp/disk.img /mnt -t vfat -o loop=/dev/loop3	# 将回环设备loop3对应到文件disk.img，然后在/mnt上挂载这个设备
mount /tmp/disk.img /mnt -o loop	# 使用了-o loop选项但没有指定具体的回环设备，将会试着分配一个未使用的回环设备
```
