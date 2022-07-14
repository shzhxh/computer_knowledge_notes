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

5. 文件管理器里添加书签：Ctrl+D

6. 禁用tracker-*

   tracker-*进程主要是为文件、应用等创建索引，但经常导致CPU、IO占用过高使系统卡顿，所以要禁用它们。

   ```
   # 方法一，禁止tracker-*程序启动
   sudo vim /etc/xdg/autostart/tracker-* # 把X-GNOME-Autostart-enabled的true改为false
   tracker reset --hard	# 结束所有的的tracker进程，删除所有的索引和配置
   tracker daemon -t	# 结束所有的tracker进程，我感觉数据可以保留，不建议删除索引和配置数据
   # 方法二，让tracker-*不工作
   	# 使用gsettings或dconf-editor，在org.freedesktop.Tracker.Miner.Files下，让crawling-interval值为-2，意为不对数据库进行检查；让enable-monitors的值为false。
   tracker reset --hard
   ```

7. gnome-shell

   Gnome Shell 是 GNOME 3 的 主要元件，它改革了 GNOME 桌面环境 的 操作，包括窗口管理、应用程序启动和工作空间管理 的 方式，为使用者提供一个全新方便易用 的 桌面管理方式。 gnome shell 本质上来说，是窗口管理器、应用启动器、桌面布局 的 集合。

    GNOME Shell 扩展就是那些可以安装在 GNOME 之上的第三方附加组件和插件。这些扩展程序是为执行特定任务而创建的，例如显示天气状况、网速等。大多数情况下，你可以在顶部面板中访问它们。也有一些 GNOME 扩展在顶部面板上不可见，但它们仍然可以调整 GNOME 的行为。

   ```
   sudo apt install gnome-tweak-tool	# 管理gnome-shell的工具
   gnome-shell --version				# 查看gnome-shell的版本
   sudo apt install gnome-shell-extensions	# 安装一些gnome-shell扩展，然后在Tweaks里控制它们的开启
   gnome-shell-extension-gsconnect		# 安装gsconnect，可以在Tweaks里控制它的开启
   gnome-extentions install 
   ```

8. 不进入图形界面

   ```
   sudo systemctl set-default multi-user.target	# 默认不进入图形界面
   sudo systemctl start gdm3.service		# 从控制台进入图形界面
   sudo systemctl set-default graphical.target		# 默认进入图形界面
   ```

   


### 注意事项

- INTEL 7960X处理器，主板Asus Strix X299-E Gaming机器在安装的时候会出现`acpi error`的提示信息且会重启导致不能正常安装，需要在启动界面为内核传入参数`acpi=off`。

  ```
  # 1. 进入grub选择界面的时候，按"e"键进入编译界面
  # 2. 在以"linux"开头的那一行未尾加上"acpi=off"
  # 3. 按"Ctrl+x"进入操作系统
  ```

  

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

  

- 关于ACPI Error的一些信息

  [对ACPI的介绍](http://www.cnblogs.com/kuwoyidai/archive/2010/08/18/1870471.html)

  [ubuntu 16.04 on x299 matherboard](https://askubuntu.com/questions/953648/error-of-first-installation-ubuntu-16-04-3-on-asus-tuf-x299-mark-2-atx-lga2066-m)
  [acpi error with ubuntu 16.04](https://askubuntu.com/questions/948386/acpi-error-while-installing-ubuntu-16-04-on-dual-boot-with-windows-10)