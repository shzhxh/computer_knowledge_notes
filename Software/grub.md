#### 安装grub

```shell
sudo mount /dev/sda1 /mnt
sudo  grub-install --target=x86_64-efi --efi-directory=/mnt
sudo update-grub
```



#### 简单配置

1. `grub-mkconfig`可以用来生成grub.cfg文件。但是功能有限。
2. /etc/default/grub文件控制`grub-mkconfig`命令的操作。
   - **GRUB_DEFAULT**：定义菜单入口，从0开始。
   - **GRUB_HIDDEN_TIMEOUT**：定义在显示菜单之前等待的时间。
   - **GRUB_TIMEOUT**：菜单等待时间，-1则会一直等待。
   - **GRUB_DISTRIBUTOR**：由grub的分发者定义的标识名，用以输出更多的菜单条目信息。
   - **GRUB_TERMINAL**：为GRUB_TERMINAL_INPUT和GRUB_TERMINAL_OUTPUT赋相同的值。指定终端输入和输出的设备。其值取决于特定平台，但一般会包含`console`本地终端、`serial`串口终端，默认为本地终端。
   - **GRUB_CMDLINE_LINUX**：添加到菜单入口的LINUX内核命令行参数
   - **GRUB_CMDLINE_LINUX_DEFAULT**：除非将GRUB_DISABLE_RECOVERY设置为`true`，否则每个内核都会生成两个入口：默认入口和覆盖模式。此命令的作用就是在GRUB_CMDLINE_LINUX后，只在默认模式下添加命令行参数。
3. 编辑/etc/grub.d文件夹中的文件或创建/boot/grub/custom.cfg，可以进行更灵活的配置。
4. 直接修改grub.cfg文件是不建议的。

#### 使系统开机进入纯命令模式

1. 编辑/etc/default/grub

   注释掉GRUB_CMDLINE_LINUX_DEFAULT，使显示详细的启动过程。

   修改GRUB_CMDLINE_LINUX为"text"

   取消对GRUB_TERMINAL的注释

2. 使用`sudo update-grub`命令使生效

3. 使用`sudo systemctl set-default mlti-user.target`命令，创建符号链接/etc/systemd/system/default.target，其链接的源文件是/lib/systemd/system/multi-user.target。

4. 重启登陆


#### 从命令模式转到图形模式

1. 编辑/etc/default/grub

   取消对GRUB_CMDLINE_LINUX_DEFAULT的注释。

   修改GRUB_CMDLINE_LINUX为""

   注释掉GRUB_TERMINAL

2. 使用`sudo update-grub`命令使生效

3. 使用`sudo systemctl set-default graphical.target`命令，创建符号链接/etc/systemd/system/default.target，其链接的源文件是/lib/systemd/system/graphical.target。

4. 重启登陆