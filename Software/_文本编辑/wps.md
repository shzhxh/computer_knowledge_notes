#### 简介

在WIN，LINUX和MAC上都是一套代码编译出来的。

源码大部分是C++，还有少量Qt和少量脚本。

#### 安装wps

```bash
# for arch linux
git clone https://aur.archlinux.org/wps-office.git
cd wps-office
wget http://kdl1.cache.wps.com/ksodl/download/linux/a21/wps-office_10.1.0.5707~a21_x86_64.tar.xz
makepkg -si


```



#### 安装字体

Linux平台默认不带很多字体，导致排版异常。`文件/选项/视图`点击**字体替换**能把缺失的字体显示出来。

```bash
# for arch linux
git clone https://aur.archlinux.org/ttf-wps-fonts.git
cd ttf-wps-fonts
makepkg -si

# for ubuntu and debian
sudo apt install ttf-mscorefonts-installer

# 适用于所有Linux系统
sudo cp -r fonts_dir /usr/share/fonts	# 直接把字体文件夹复制到对应目录下即可
```

#### 审阅

review -> track changes -> track changes开启审阅。

#### 问题解决

1. 无法输入中文

   参考此链接[wps使用fcitx5无法输入中文](https://wiki.archlinux.org/title/WPS_Office_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#Fcitx5_%E6%97%A0%E6%B3%95%E8%BE%93%E5%85%A5%E4%B8%AD%E6%96%87)，配置完成后需要重启系统。
