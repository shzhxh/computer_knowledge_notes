#### 简介

开源的网络探测和安全审核工具。

#### 格式

```
nmap [扫描类型] [通用选项] {描述目标}
```

#### 扫描类型

```
-sT
-sS
-sF,-sX,-sN
-sP
-sU
-sA
-sW
-sR
-b
```

#### 通用选项

```
-P0
-PT
-PS
-PI
-PB
-O
```

#### 扫描目标

```
目标地址
-iL filename
-iR
-p
-exclude
-excludefile
```

#### 使用示例

```
sudo nmap -sn -PT -PI 192.168.1.0/24	
	# 扫描网段内所有ip,-sn的意思是不扫描端口,-PT通过发TCP echo包确定主机是否在线，-PI通过发ICMP echo包确定主机是否在线
```