#### 语法

```bash
mount [-l|-h|-V]
mount -a [-fFnrsvw][-t fstype][-O optlist]	# 挂载fsbab里记录的所有文件系统
mount [-fnrsvw][-o options] device|dir
mount [-fnrsvw][-t fstype] [-o options] device dir
```

#### 选项

```
-a	# 挂载fsbab里记录的所有文件系统
-B
-c
-F
-f
-i
-L
-l	# 显示已加载的文件系统
-M
-n
-O
-o	# 
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



#### 示例

```bash
mount -t type device dir	# mount的标准形式，将设备device上type类型的文件系统挂载到目录dir上
mount /tmp/disk.img /mnt -t vfat -o loop=/dev/loop3	# 将回环设备loop3对应到文件disk.img，然后在/mnt上挂载这个设备
mount /tmp/disk.img /mnt -o loop	# 使用了-o loop选项但没有指定具体的回环设备，将会试着分配一个未使用的回环设备
```
