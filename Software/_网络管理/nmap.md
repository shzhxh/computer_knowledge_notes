#### 简介

开源的网络探测和安全审核工具。

#### 格式

```
nmap [扫描类型] [通用选项] {描述目标}
```

#### 主机发现

```
-sn	# ping扫描，不扫描端口
```

#### 扫描技术

```

```

#### 端口指定与扫描次序

```
```

#### 服务/版本检测

```
```

#### 脚本扫描

```
```

#### OS检测

```
```

#### 计时与性能

```
```

#### 防火墙/IDS

```
```

#### 输出

```
-v	# 详细模式，-vv可以得到更详细的信息
```

#### 其它

```
```

#### 扫描目标

```
主机名、IP地址、网络	# 如scanme.nmap.org, microsoft.com/24, 192.168.0.1, 1.0.0-255.1-254
-iL filename
-iR
-p
-exclude
-excludefile
```

#### 使用示例

```
nmap -v scanme.nmap.org	# 扫描scanme.nmap.org的所有保留的TCP端口
nmap -sS -O scanme.nmap.org/24
sudo nmap -sn 192.168.1.0/24	
	# 扫描网段内所有ip,-sn的意思是不扫描端口
```