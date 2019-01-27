```bash
# 创建加密分区
sudo cryptsetup --verbose --verify-passphrase -c aes-cbc-plain luksFormat /dev/sdc1

# 把加密分区映射为back
sudo cryptsetup luksOpen /dev/sdb1 back

# 挂载使用
sudo mkfs.ext4 /dev/mapper/back
sudo mount /dev/mapper/back /mnt/back/
sudo cp XXX /mnt/back/

# 关闭映射
sudo umount /mnt/back
sudo cryptsetup luksClose /dev/mapper/back
```
