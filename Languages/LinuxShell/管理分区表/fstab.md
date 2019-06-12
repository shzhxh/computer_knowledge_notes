#### 简介

描述可挂载文件系统的信息。

#### 各个域

##### file system

指挂载的文件系统。其形式主要有三种：块设备(/dev/sdb7)，LABLE=，UUID=。

##### mount point

指文件系统的挂载点。对于交换分区来说则是none。

##### type

指文件系统的类型。

##### options

指文件系统的挂载选项。

- defaults：使用默认选项：rw, suid, dev, exec, auto, nouser,
                       async.
- async：应异步完成所有对文件系统的I/O。
- sync：文件系统的所有I/O都应该同步地完成。
- rw：挂载的文件系统可读写。
- ro：挂载的文件系统只读。
- suid：允许set-user-ID 或 set-group-ID 位生效。
- dev：解释文件系统上的字符设备或块设备。
- exec：允许执行二进制文件。
- auto：可以使用-a选项来挂载。
- noauto：使用`mount -a`时不挂载。
- user：允许用户挂载。
- nouser：禁止用户挂载文件系统。
- owner：允许设备所有者挂载。
- nofail：即使设备不存在也不报告错误。

##### dump

由dump使用，以决定文件系统是否需要备份。默认为0(不复制)。1则代表每天都进行dump操作，2代表不定期进行dump操作。

##### pass

由fsck使用，以确定在启动的时候文件系统检查的顺序。根文件系统应设置为1,其它文件系统应设置为2.默认为0(不检查).