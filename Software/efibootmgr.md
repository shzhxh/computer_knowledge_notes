#### 作用

控制EFI启动管理器

#### 选项

```
-a	# 激活bootnum
-A	# 不激活bootnum
-b	# 修改BootXXX
-B	# 删除bootnum
-c	# 创建bootnum并添加到bootorder
-d	# 此盘包含loader(默认/dev/sda)
-D	# 删除重复的条目
-e	# 
-E	# 
-g	# 
-i 	# 创建网络启动项
-l 	# 定义loader(默认elilo.efi)
-L	# 启动管理器的标志(默认为Linux)
-m	# 
-M	#
-n	# 设置BootNext
-N	# 删除BootNext
-o	# 设置BootOrder
-O	# 删除BootOrder
-p	# 包含bootloader的分区号(默认为1)
-q	# 安静模式
-r
-t 	# 设置timeout，单位为秒
-T	# 删除Timeout变量
-u	#
-v	# Verbose模式
-V	# 打印版本信息
-w
-y
-@
```



#### 用法示例

```shell
efibootmgr	# 显示当前设置，必须是root用户
	# BootCurrent: 0004	，当前启动项是0004
    # BootNext: 0003， 下次启动时启动项为0003,此项在使用完毕后将被管理器删除
    # BootOrder: 0004,0000,0001,0002,0003，启动顺序
    # Timeout: 30 seconds，启动管理器在屏幕上显示的时间为30秒
    # Boot0000* Diskette Drive(device:0)
    # Boot0001* CD-ROM Drive(device:FF)
    # Boot0002* Hard Drive(Device:80)/HD(Part1,Sig00112233)
    # Boot0003* PXE Boot: MAC(00D0B7C15D91)
    # Boot0004* Linux
efibootmgr -c	# 创建新的启动项
efibootmgr -o 3,4	# 创建启动顺序 0003,0004
efibootmgr -n 4		# 下次启动从0004开始，仅一次有效
efibootmgr -b 4 -B  # 删除启动项4,并把它从BootOrder中删除
efibootmgr -c -i eth0 -L netboot [-l '/filename.efi']	# 创建网络启动项
```



