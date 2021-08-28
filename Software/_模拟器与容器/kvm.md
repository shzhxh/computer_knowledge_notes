#### for ubuntu
```shell
sudo apt-get install qemu-kvm libvirt-bin virt-manager virtinst bridge-utils cpu-checker
```

#### for arch linux

```bash
# 检查支持
lscpu	# 硬件检查，存在Virtualization项
zgrep KVM /proc/config.gz	# 内核检查，模块设置等于y或m

# 安装
pacman -S qemu virt-manager bridge-utils
```

#### 错误处理

1. Verify that 'libvirtd' daemon is running

   ```bash
   sudo service libvirt-bin start	# for ubuntu
   systemctl start libvirtd.service	# for arch
   ```

   ​