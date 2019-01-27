#### 当不能从openthos自带的引导器启动的时候。。。

##### 解决方法：从grub引导

1. 编辑`/etc/grub.d/40_custom`，添加如下代码

   ```
   menuentry "openthos"{
       set root=(hd0,1)
       linux /Openthos/kernel quiet loglevel=0 root=/dev/ram0 androidboot.selinux=permissive BOOT_MODE=hdboot RAMDISK_HD_UUID= RAMDISK_IMG=Openthos/ramdisk.img SYSTEM_HD_UUID= DATA_HD_UUID=
       initrd /Openthos/initrd.img
   }
   ```

   第一行指定根目录所在的分区，第二行指定内核为kernel，后面跟着的都是参数，可以看到它指定了openthos的三个分区，第三行是指定内存中的文件系统，辅助内核启动。

2. 执行命令`sudo update-grub`，相当于`sudo grub-mkconfig -o /boot/grub/grub.cfg`，重新为grub生成配置文件。`grub-mkconfig`命令默认是将配置输出到标准输出的。