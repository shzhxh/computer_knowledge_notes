#### 目录结构

目录配置：（FHS)(四种类型：static,variable,shareable,unshareable)

```
/bin	//常用命令
/sbin	//系统管理员才会用到的命令
/lib	//开机及常用命令的函数库
/usr	//unix software resouce:软件放置处
/opt	//第三方软件
/srv	//网络服务
/tmp	//放临时文件
/etc	//配置文件
	/etc/passwd	//用户信息
	/etc/shadow	//密码信息
	/etc/group	//组信息	
/boot	//开机与内核
/dev	//设备文件
/var	//variable:与系统动作过程有关
/media	//放置可移除的设备
/mnt	//暂时挂载
/home	//用户目录
/root	//系统管理员目录
/lost+found	//文件系统发生错误时，放置遗失片断
/proc	//内存中数据
/sys	//内核相关信息
```

#### 文件种类

文件三种权限：rwx

```
d	//directory文件
-	//一般文件
l	//link文件
b	//block设备，一般在/dev目录下
c	//character设备，一般在/dev目录下
s	//socket文件，例/var/run目录下
p	//数据输送文件
```



#### 应用程序

1. 一些自己安装的应用程序在Dash上是搜索不出来的，那么怎么样才能在Dash上搜索到指定的应用程序呢？

   Dash上的应用程序都在`/usr/share/applications`里，依照其它`.desktop`文件的格式自己创建一个即可。

2. 安装输入法

   ```bash
   # for ubuntu 16.04
   sudo apt install im-config fcitx-table-wbpy fcitx-pinyin
   im-config -s fcitx -z all_ALL

   # for archlinux
   pacman -S fcitx fcitx-im fcitx-table-extra fcitx-configtool
   ```

   ​

#### 硬盘分区

1. 应当将读写频繁的目录分离出来，以保护固态硬盘
   - 感觉`opt`目录没必要单独分出来，里面基本不放什么程序。即使新安装什么程序也可以自己指定，不必一定要安装在`opt`目录。
   - 感觉`home`目录没必要单独分出来，即使重装了系统，`home`目录下的配置也是不一样的。应该多划分一个分区保存数据，相当于`home`目录和`opt`目录的功能。
   - `tmp`，`usr`和`var`这三个应单独分区，因为它们读写频繁，单独分区可以保护固态硬盘。
2. 如果没有固态硬盘，只单独划分一个数据分区保护自己的数据就足够了。
   - 将数据分区的内容链接到home目录下。
   - 将对系统的设置保存到单独的脚本里，重装系统的时候运行脚本恢复之前的环境。

#### 库的设置

- 库的路径由文件`/etc/ld.so.conf`指定

  `include /etc/ld.so.conf.d/*.conf`，可见其具体的定义都在`/etc/ld.so.conf.d`文件夹中。

- 使用`ldconfig`命令使配置生效。