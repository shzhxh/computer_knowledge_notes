#### 简介

Celadon是Intel主导的开源项目，旨在实现Intel架构对Android的支持，其核心是一个叫AAF的东西：自动适配框架。AAF会自动检测并绑定内核驱动，载入HAL模块，载入Intel平台的设备配置。

#### 虚拟机上运行Celadon

我的宿主机上运行的是Ubuntu18.04。

```
# 1. 准备
mkdir ~/civ		# Celadon in VM
cd ~/civ
wget https://raw.githubusercontent.com/projectceladon/device-androidia-mixins/master/groups/device-specific/caas/setup_host.sh
# setup_host.sh是用来帮助做一些配置工作
chmod +x setup_host.sh
sudo -E ./setup_host.sh

# 2. 编译镜像
mkdir civ && cd civ
repo init -u https://github.com/projectceladon/manifest.git
repo sync -c
```

