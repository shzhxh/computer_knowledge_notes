#### 安装

```
sudo pacman -S nbd
cd /lib/modules/5.3.11-1-MANJARO/kernel/drivers/block
xz -z nbd.ko
sudo modprobe nbd
```

#### nbd-server

```
nbd-server [ip@]port <filename> [size] [options]	# 把文件当作块设备
```

#### nbd-client

```
nbd-client <host> [port] <nbd-device> [options]
nbd-client <-unix> <path> <nbd-device> [options]
ndb-client [-d | -c] nbd-device
nbd-client -l <host> [port]
nbd-client [-netlink] -l <host>
```

