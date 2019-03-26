##### 对Openthos不足之处的思考

1. Openthos不是多用户的OS，只有一个用户。
2. Openthos支持的应用都是适配于手机或平板的，它们的特点的小屏幕，触控操作。这和台式机上大屏幕，鼠键操作的逻辑是不一样的。
3. Openthos上应用的开发只能是android studio，而台式机上的开发环境是多种多样的。

##### 从grub引导openthos

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