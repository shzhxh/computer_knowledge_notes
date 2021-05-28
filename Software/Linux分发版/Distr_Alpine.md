#### qemu中安装alpine

```bash
	qemu-img create -f raw image_file 8G	# to create hard disk images
	qemu-system-x86_64 -m 1G -cdrom iso_image -boot order=d -net nic -net user -drive file=disk_image,format=raw	# installing the OS
	
# 用户名为root，无密码
setup-alpine

# 启动alpine
	qemu-system-x86_64 -m 1G -net nic -net user -drive file=disk_image,format=raw
```

