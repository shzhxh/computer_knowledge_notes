#### 错误解决

##### error: unknown filesystem

- 描述:在启动的时候grub直接进入恢复模式，提示"unknown filesystem"。或者在有的电脑上直接就识别不出来启动盘。

- 原因分析：问题应该是出在启动盘的grub上，因为电脑自带的grub没问题，而启动盘的grub报错。

- 解决方法：网上提到的方法是用ls命令找到manjaro所在的分区，然后用set命令进行设置。但我面临的问题是ls命令列出的分区均提示"unknown filesystem"。这种情况下该怎么办呢？有变通的方法，就是用硬盘上已有的grub来引导manjora。过程如下：

  ```
  ls 	# 应该可以看到(hd0,msdos1)，这就是manjaro启动分区
  ls (hd0,msdos1)/	# 如果可以看到存在manjaro/目录，表明确实就是这个分区
  set root=hd0,msdos1	# 设置根设备
  set prefix=(hd0,msdos1)/boot/grub	# 设置grub目录的绝对路径
  insmod normal	# 调用normal模块
  normal			# 
  ```

  

