#### 安装wps

```bash
# for arch linux
git clone https://aur.archlinux.org/wps-office.git
cd wps-office
wget http://kdl1.cache.wps.com/ksodl/download/linux/a21/wps-office_10.1.0.5707~a21_x86_64.tar.xz
makepkg -si


```



#### 安装字体

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

