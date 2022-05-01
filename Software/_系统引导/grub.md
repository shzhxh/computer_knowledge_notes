#### 安装grub

```shell
sudo mount /dev/sda1 /mnt
sudo  grub-install --target=x86_64-efi --efi-directory=/mnt
sudo update-grub
```

#### 简单配置

1. `grub-mkconfig -o grub.cfg`可以用来生成grub.cfg文件。但是功能有限。

2. /etc/default/grub文件控制`grub-mkconfig`命令的操作。
   - **GRUB_DEFAULT**：定义菜单入口，从0开始。
   
     > 注：主菜单的编号是从0开始，如果还有子菜单的情况怎么办呢？此时应该用>分隔主菜单和子菜单。比如：第1个主菜单下的第4个子菜单则表示为"1>4"。
     >
     > 注：也可以通过名字来指定默认入口。比如：GRUB_DEFAULT="1>Ubuntu, with Linux 3.13.0-24-generic"这样。
   
   - **GRUB_HIDDEN_TIMEOUT**：定义在显示菜单之前等待的时间。
   
   - **GRUB_TIMEOUT**：菜单等待时间，-1则会一直等待。
   
   - **GRUB_DISTRIBUTOR**：由grub的分发者定义的标识名，用以输出更多的菜单条目信息。
   
   - **GRUB_TERMINAL**：为GRUB_TERMINAL_INPUT和GRUB_TERMINAL_OUTPUT赋相同的值。指定终端输入和输出的设备。其值取决于特定平台，但一般会包含`console`本地终端、`serial`串口终端，默认为本地终端。
   
   - **GRUB_CMDLINE_LINUX**：添加到菜单入口的LINUX内核命令行参数
   
   - **GRUB_CMDLINE_LINUX_DEFAULT**：除非将GRUB_DISABLE_RECOVERY设置为`true`，否则每个内核都会生成两个入口：默认入口和覆盖模式。此命令的作用就是在GRUB_CMDLINE_LINUX后，只在默认模式下添加命令行参数。
   
3. 编辑/etc/grub.d文件夹中的文件或创建/boot/grub/custom.cfg，可以进行更灵活的配置。

4. 直接修改grub.cfg文件是不建议的。

#### 命令

```
boot	# 启动已加载的操作系统。
set [envvar=value]	# 将环境变量envvar的值设置为value，如不给定参数则列出当前的环境变量
unset envvar	# 释放环境变量envvar
lsmod	# 列出已加载的模块
insmod	# 调用指定的模块
linux	# 装载指定的内核文件，并传递内核启动参数。
initrd	# 只能紧跟在linux命令之后，为指定的内核传递init ramdisk路径。
search	# 通过文件(--file)、卷标(--label)、UUID(--fs-uuid)来搜索设备。
ls [args]	# 如不给定参数，则列出所有grub可见的设备；如给定的参数是一个分区，则显示该分区的信息；如给定的参数是一个绝对路径，则显示该路径下的所有文件。 
```

#### 内置变量

```
cmdpath	# core.img所在目录的绝对路径，该绝对路径要包含设备名称
default	# 默认的菜单项。可使用菜单的titile, id 或 数值顺序。
timeout	# 设置菜单等待时间。如为0则直接启动default指定的菜单，如为-1则永久等待手动选项。
fallback	# 如默认菜单项启动失败，则使用该变量指定的菜单项启动。
prefix	# grub目录的绝对路径，这样才可以从该目录下加载各文件和模块
root	# 指定根设备的名称
```



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