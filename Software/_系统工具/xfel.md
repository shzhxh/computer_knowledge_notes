#### 概述

用于全志SOC的FEL工具。FEL是全志设备的BootROM里的一个程序，用USB进行初始编程和设备恢复。

#### 安装

```
sudo apt install libusb-1.0-0-dev
git clone https://github.com/xboot/xfel.git
cd xfel
make
sudo make install
```

#### 用法

```bash
xfel version	# 显示芯片版本
xfel hexdump <address> <length>	# 以16进制显示内存
xfel dump <address> <length>	# 以2进制显示内存
xfel exec <address>		# 调用程序地址
xfel read32 <address>	# 从设备内存读32位值
xfel write32 <address> <value>	# 把32位值写入设备内存
xfel read <address> <length> <file>	# 读内存到文件
xfel write <address> <file>	# 写文件到内存
xfel reset	# 使用watchdog重置设备
xfel sid	# 显示sid信息
xfel jtag	# 启用jtag调试
xfel ddr [type]	# 初始化ddr控制器
xfel spinor		# 删除spi nor闪存
xfel spinor erase <address> <length>	# 删除spi nor闪存
xfel spinor read <address> <length> <file>	# 把spi nor闪存写入文件
xfel spinor write <address> <file>	# 把文件写入spi nor闪存
xfel spinand		# 删除spi nand闪存
xfel spinand erase <address> <length>	# 擦除spi nand闪存
xfel spinand read <address> <length> <file>	# 把spi nand闪存写入文件
xfel spinand write <address> <file>	# 把文件写入spi nand闪存
xfel spinand splwrite <split-size> <address> <file>
	# 把文件写入支持split的spi nand闪存
```

#### 示例

```
# 烧写SPI NAND闪存
xfel spinand
xfel spinand erase 0x000000 134217728
xfel spinand write 0x000000 u-boot-sunxi-with-nand-spl.bin 
xfel spinand write 0x80000 splash.bmp 
xfel spinand write 0x100000 kernel.itb 
xfel spinand write 0x600000 rootfs.squashfs
```

