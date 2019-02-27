#### 简介

Android Sdk工具，在`platform-tools/fastboot`目录下。

#### 语法

```
fastboot [option] <command>
```

#### 命令

```
boot <kernel> [<ramdisk> [<second>]]		# 下载并启动kernel，此操作只对本次启动有效，下次启动依然和以前一样
devices [-l]								# 列出所有连接过来的设备
flash <partition> [<filename>]				# 将文件filename写入分区partition
reboot
```



#### 选项

```
-w				# 删除用户数据和缓存(如果分区类型支持的话还会进行格式化)
```

#### 示例

```
fastboot boot boot.img		# 用当前目录下的boot.img启动android
fastboot boot recovery.img	# 用当前目录下的recovery.img启动android到recovery模式
fastboot flash boot boot.img	# 把当前目录下的boot.img刷入手机的boot分区
```

#### 错误提示

##### no permissions (verify udev rules)

- 现象：使用`fastboot devices`出现提示no permissions (verify udev rules)

- 原因分析：因为ubuntu是在非root身份运行，要使用usb调试，需要设置usb权限

- 解决方法

  ```
  cd /lib/udev/rules.d
  sudo vim 61-persistent-storage-android.rules
      # 添加ACTION!="remove", KERNEL=="mmcblk[0-9]p[0-9]", ENV{PARTNAME}=="?*", SYMLINK+="disk/by-partlabel/$env{PARTNAME}"
  ```

  