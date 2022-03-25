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

# for ubuntu
sudo apt install ttf-mscorefonts-installer

# 适用于所有Linux系统
sudo cp -r fonts_dir /usr/share/fonts	# 直接把字体文件夹复制到对应目录下即可
```

