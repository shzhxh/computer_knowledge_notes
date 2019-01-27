#### for arch linux

```bash
git clone https://aur.archlinux.org/wps-office.git
cd wps-office
wget http://kdl1.cache.wps.com/ksodl/download/linux/a21/wps-office_10.1.0.5707~a21_x86_64.tar.xz
makepkg -si

# 安装字体
git clone https://aur.archlinux.org/ttf-wps-fonts.git
cd ttf-wps-fonts
makepkg -si
```

