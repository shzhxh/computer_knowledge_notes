##### 从grub命令行启动并修复grub

```
ls		# 通过此命令找到deepin所在分区，假定在(hd0,gpt1)
set
linux (hd0,gpt1)/vmlinuz root=/dev/sda1 foo bar		# 加载内核
initrd (hd0,gpt1)/initrd.img
boot	# 进入deepin系统

# 在deepin系统中开启一个终端
sudo update-grub
```

