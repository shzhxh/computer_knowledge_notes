#### 参考资料

- [安装说明官方文档](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)


- [树莓派3自编手册](https://pan.baidu.com/s/1mhDWoCo?spm=a1z09.2.0.0.5w50lW#list/path=%2F)
- [树莓派书籍](https://pan.baidu.com/s/1i4WZlRJ?spm=a1z09.2.0.0.5w50lW#list/path=%2F )
- [树莓派源](http://www.raspbian.org/RaspbianMirrors)

#### 安装过程

1. 准备安装介质：

		下载raspbian,使用sha1sum验证sha1值
	注：使用wget可以获得很好的下载速度

2. 安装操作系统：

```shell
df -h		/找到tf卡的硬件名/
dd bs=4M if=raspbian.iso of=/dev/mmcblk0
```

3. 刻好的卡里设置wifi：

	在etc/wpa_supplicant/wpa_supplicant.conf的末尾添加
	network={
	ssid="testing"
	psk="testingPassword"
	}
4. ssh:

在boot分区内`touch ssh`创建空文件ssh

5. 更新源：

```shell
# 在etc/apt/sources.list注释掉原来的源，添加
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ jessie main non-free contrib
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ jessie main non-free contrib
sudo apt-get update
sudo apt-get upgrade
```

#### 共享打印机

```
sudo apt update
sudo apt install hplip
sudo apt install cups

sudo usermod -a -G lpadmin pi
sudo cp /etc/cups/cupsd.conf /etc/cups/cupsd.conf.bak	# 备份
sudo vim /etc/cpus/cupsd.conf
	# 将Localhost:631改为0.0.0.0:631
	# 在Location /, /admin, /admin/conf 下添加"Allow all"
	
wget http://foo2zjs.rkkda.com/foo2zjs.tar.gz
tar xf foo2zjs.tar.gz
cd foo2zjs
make
./getwdb P1005
sudo make install
sudo make install-hotplug
sudo make make cups

# 浏览器登陆树莓派ip:631，添加打印机

# 重启树莓派
```




