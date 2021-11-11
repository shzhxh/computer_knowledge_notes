> 系统：Ubuntu 18.04.6
>
> 内核版本：5.4.0-87-generic

#### 1 下载内核与补丁

```
# 内核下载地址：https://mirrors.edge.kernel.org/pub/linux/kernel/
# 补丁下载地址：https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/
# 通过与本地内核版本对比，最接近的应该是5.4.154，故下载此版本的内核与补丁
mkdir rt_kernel && cd rt_kernel
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.154.tar.xz
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.154-rt65.patch.xz
tar xf linux-5.4.154.tar.xz
cd linux-5.4.154
xzcat ../patch-5.4.154-rt65.patch.xz | patch -p1
```

#### 2 配置内核

```
cp /boot/config-5.4.0-87-generic linux-5.4.154/.config
sudo apt install libncurses-dev bison flex bc libelf-dev libssl-dev
make oldconfig	# 选Fully Preemptible Kernel (RT)
```

#### 3 编译内核

```
vim .config		# 可注释掉CONFIG_DEBUG_INFO，以减少内核大小
make -j4 deb-pkg	# 我的CPU是4核，故选-j4
```

##### 错误解决

```
##### 错误1
# 错误描述：No rule to make target 'debian/canonical-certs.pem', needed by 'certs/x509_certificate_list'
# 解决方法：编辑.config，将CONFIG_SYSTEM_TRUSTED_KEYS置为空。
```

#### 4 安装deb包

```
sudo dpkg -i ../*.deb
```

