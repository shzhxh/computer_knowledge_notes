### 快速开始

#### 通过web接口访问OpenWrt

浏览器里输入IP地址访问LuCI web接口。对于路由器来说，默认地址`192.168.1.1`，管理账户是`root`。

#### 开启一个Wi-Fi访问点

##### 使用web接口

1. 连接到web接口

2. 进入`Network/wireless`。每个无线设备都对应自己的Wi-Fi配置部分(大部分设备都会有2.4GHz和5GHz两个无线设备)。

3. 对于每个无线设备，点击`Add`创建Wi-Fi网络：

   - 在第一个Wi-Fi网络，进到`Device Configuration`并打开`Advanced Settings`标签：在`Country Code`字段，选择正确的国家代码。它的重要性在于使你的OpenWrt满足你的国家的法律。(其它无线设备将使用同样的配置)

   - 对于每个无线网络，在`Interface Configuration`，设置你的Wi-Fi：

     在`General Setup`，在`ESSID`设置Wi-Fi网络的名称

     在`Wireless Security`，选择一个`Encryption`方法(比如，建议家庭/小型办公网络使用WPA2-PSK)

     在`Wireless Security`，选择一个`Cipher`(比如，建议`auto`)

     在`Wireless Security`，输入一个`key`，它是登入无线网络的密码。对于WPA2-PSK来说，密码不得低于八个字符。

     其它的默认配置对于一般的使用就足够了，但也可以根据需要进行更进一步的配置。

4. 完成后点击`Save & Apply`。

错误诊断：

- 5GHz Wi-Fi看上去没有启动的问题：如果你的Wi-Fi信道>100，在能使用这些信道之前，设备必须先扫描它们。这大概需要1-10分钟，取决于Wi-Fi情况和信道数量。在每次自动信道变更后也会体验到1分钟的延迟，也是因为同样的扫描延迟。
- 在`General Setup/Network`应选择`lan`或激活的DHCP服务器，不要选择`wan`或`wan6`(因为它们是仅用于互联网连接的上游接口)。

##### 使用命令行

```
uci show wireless	# 看所有的无线配置，配置里的radio指的是设备上的Wi-Fi芯片
# 在“ISO/IEC 3166 alpha2”里找到国家代码。
uci set wireless.radio0.country='XX'	# 为radio0设置国家代码
```



#### 走向OpenWrt安装

#### Factory安装：设备上的第一次安装

Factory安装指的是第一次刷机的安装过程，在刷机完成后使用"系统升级"来更新固件。

#### 安装OpenWrt开发快照

开发快照也被简单地称为“快照”或过时的术语“主干构建”。快照是“正在开发中”的OpenWrt版本。他们经常重建，通常一天要重建好几次。

#### Internet连接与故障排除

#### 登陆运行着OpenWrt的路由器

登陆后，将看到状态页。这里可以概览路由器状态。

`System`块是一些基本的系统信息，如OpenWrt版本和web接口的名称(LuCI)，还有重启之后的运行时间，路由器的时钟时间，处理器的使用率。

`Memory`块是内存使用。如果可用内存很低，路由器将变慢并表现的不稳定，这种情况下就需要关闭一些服务。

`Network`块显示的是网络接口的信息。

`DHCP Leases`块显示的是DHCP分配的计算机。显示的是用户计算机所分配到的临时IP地址。

`Wireless`块显示的是无线网络信息。出于安全的原因，在初始模式下它们都是关闭的，需要手动打开。

#### OpenWrt替补列表搜索

如果在标准的刷机指导上没有提供你要的信息，请从论坛或OpenWrt网站上查找你的设备的信息，以找到合适的OpenWrt固件镜像。

#### 新用户FAQ

#### 安全性与开启Wi-Fi

与[开启一个Wi-Fi访问点](#开启一个Wi-Fi访问点)内容基本相同

#### 新用户的SSH访问

```
ssh root@192.168.1.1
```



#### 诊断网络连接

#### 通过web接口更新OpenWrt

#### 通过SSH连接到internet，并安装Luci Web接口

```
# 通过连接到ISP猫来上网
uci set network.lan.ipaddr='192.168.2.200'	# 给OpenWrt设备分配IP
uci set network.lan.gateway='192.168.2.1'	# 将网关设定为ISP猫的IP
uci set network.lan.dns='192.168.2.1'		# 将dns设定为ISP猫的IP
uci commit
service network restart
# 设置完成后，需要使用192.168.2.200来登陆OpenWrt设备

# 安装LuCI
opkg update
opkg install luci
```



#### 验证OpenWrt固件二进制

通过计算固件二进制的checksum，来避免下载错误。

#### WebUI系统更新：“Keep settings”复选框

“保留设置”复选框是一个更高级的功能。它在升级前执行OpenWrt备份，升级后恢复。

如果你不能准确的理解这个按钮的适用情况，那么在每次刷写固件的时候就不要选择它，这样就不会保留设置。

### 用户手册

#### 安装

#### 基本配置

#### LuCI web接口

#### 网络配置

#### 固件配置

#### 高级配置

#### 安装其它软件

#### 特定于硬件的配置

#### 存储设备

#### 附加服务

#### 故障诊断与维护

#### 在虚拟机或容器里运行OpenWrt

#### 安全

#### 版本的签名

#### 半成品文档页