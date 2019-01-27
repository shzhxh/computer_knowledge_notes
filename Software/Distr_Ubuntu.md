### INSTALL

ubuntu下使用程序Startup Disk Creator 创建启动U盘。

1. 选择uefi+gpt方式，不要选bios+mbr方式。
2. disable secure mode。
3. 根目录放ssd分区提升开机速度，读写频繁的目录放hdd分区保护ssd。
4. 新安装好的ubuntu在炫龙笔记本的显卡驱动有问题，需要在恢复模式下安装好正确的显卡驱动。
5. 在命令模式下输出`^@`是由ACPI引起的，可以通过给内核传递`acpi=off`参数解决此问题
### 错误解决

- System program problem detected

  ```
  sudo rm /var/crash/*	# 删除奔溃信息
  vim /etc/default/apport	# 设置调试工具Apport失效，使弹窗永久失效
  ```

- activation of network connection failed

  原因未知，观察到没有有线网络，使用`ifconfig`命令只能看到inet6的信息，而没有inet的信息。基于以上信息，猜测是有线模块配置的问题，ipv4配置错误使联网失败。

  暂无解决方法，也许可以重启试试。重启后问题解决。

- failed to start LSB: QEMU KVM module loading script

  `sudo apt-get purge qemu-system-common`

- sh: 1: arm-none-eabi-gcc: not found

  arm-none-eabi-gcc明明是存在的，系统却提示not found。这是因为arm-none-eabi-gcc是32位的程序，而我的系统却是64位的系统。要让64位的系统执行32位的程序，需要执行如下命令：

  ```
  sudo dpkg --add-architecture i386
  sudo apt update
  sudo apt install libc6:i386 libncurses5:i386 libstdc++6:i386
  ```


### CONFIG

1. 中文输入法:fcitx

2. 在`Software & updates`里改源，清华的源较快

3. 需要安装的软件

   ```shell
   # 开发
   sudo apt install eclipse-cdt default-jdksud
   # 画图
   sudo apt install umbrello dia
   # 格式转化
   sudo apt install pandoc
   ```

4. 安装指定版本的软件

   ```bash
   add-apt-repository 'deb http://archive.ubuntu.com/ubuntu trusty main'
   apt-get update
   apt-cache search llvm | grep 3.4
   apt install llvm-3.4
   ```

   ​


### 注意事项

- INTEL 7960X处理器，主板Asus Strix X299-E Gaming机器在安装的时候会出现`acpi error`的提示信息且会重启导致不能正常安装，需要在启动界面为内核传入参数`acpi=off`。

  ```
  # 1. 进入grub选择界面的时候，按"e"键进入编译界面
  # 2. 在以"linux"开头的那一行未尾加上"acpi=off"
  # 3. 按"Ctrl+x"进入操作系统
  ```

  ​

- INTEL机器在安装完成后依然需要在启动选项里加入`acpi=off`参数。

  ```
  # 1. 第一次启动的时候也要手动修改才能进入ubuntu
  # 2. 进入系统后执行如下操作
  sudo vim /etc/default/grub
  	# 在GRUB_CMDLINE_LINUX=后面添加"acpi=off"
  	# 编辑完毕，保存并退出vim
  # 3. 执行如下操作，更新启动项
  sudo update-grub
  ```

  ​

- 关于ACPI Error的一些信息

  [对ACPI的介绍](http://www.cnblogs.com/kuwoyidai/archive/2010/08/18/1870471.html)

  [ubuntu 16.04 on x299 matherboard](https://askubuntu.com/questions/953648/error-of-first-installation-ubuntu-16-04-3-on-asus-tuf-x299-mark-2-atx-lga2066-m)
  [acpi error with ubuntu 16.04](https://askubuntu.com/questions/948386/acpi-error-while-installing-ubuntu-16-04-on-dual-boot-with-windows-10)