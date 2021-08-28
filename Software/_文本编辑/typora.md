在https://support.typora.io/获取帮助。

#### 在Debian/Ubuntu下安装

```shell
# optional, but recommended
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

# add Typora's repository
sudo add-apt-repository 'deb https://typora.io ./linux/'
sudo apt-get update

# install typora
sudo apt-get install typora
```

#### 下载二进制安装

1. 下载https://typora.io/linux/typora_0.9.96_amd64.deb
   > 或者下载https://typora.io/linux/Typora-linux-x64.tar.gz，可以使用离线下载
2. 解压后，双击里面的Typora文件即可运行
3. 可以在/usr/share/applications文件夹里创建一个desktop文件，使其可以在启动菜单中显示


#### 在archLinux下安装

```bash
# 回忆版，大致流程如下，但网址不准确
git clone aur.archlinux.org/typora.git
cd typora
wget typora.io/typora.deb
make install
```

#### Markdown语法

下标：File > Preference > Markdown > Syntax Support开启。

[emoji](https://www.webfx.com/tools/emoji-cheat-sheet/)

[数学符号](https://blog.csdn.net/weixin_43159148/article/details/88621318)
