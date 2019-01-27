参考：https://www.cnblogs.com/hfyfpga/p/4619900.html

#### 基本思路

最终想实现的效果是一个U盘可以安装多个系统。为实现此目标，应在U盘上安装grub，gub再引导各个ISO文件。

#### 安装过程

把casper下的vmlinux和initrd.lz复制出来。

修改grub.cfg文件，引导多个安装镜像。