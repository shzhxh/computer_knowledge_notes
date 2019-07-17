#### 概述
记录在两台主机上安装Ubuntu17.10 server(**命令行**)和Ubuntu 16.04 Desktop(**图形界面**)的情况。一台主机上是AMD 1950X处理器，主板Asus X399-A。另一台是INTEL 7960X处理器，主板Asus Strix X299-E Gaming。

#### 分区情况
均使用的是**UEFI**分区，有两块硬盘，一块8T机械硬盘，一块256G固态硬盘。

- AMD机器：固态硬盘上安装的是启动分区和**ubuntu17.10**，机械硬盘划分了**7个200G**的分区和**1个4T**的分区，其它空间未分配。在机械硬盘的一个200G分区上安装了**ubuntu16.04**。4T分区挂载在ubuntu的**/data**目录下。
- INTEL机器：固态硬盘上安装的是**Windows10**(未激活)，机械硬盘划分了**8个200G**的分区和**1个4T**的分区，其它空间未分配。在机械硬盘的两个200G分区上分别安装了**ubuntu17.10**和**ubuntu16.04**。4T分区挂载在ubuntu的**/data**目录下。

#### 注意事项
- INTEL机器在安装的时候会出现`acpi error`的提示信息且会重启导致不能正常安装，需要在启动界面为内核传入参数`acpi=off`。

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
