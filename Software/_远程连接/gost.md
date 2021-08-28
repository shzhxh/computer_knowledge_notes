#### 选项

```
-C <string>	# 配置文件
-D	# 开启调试日志
-F <value>	# 设置转发地址，可做成转发链
-L <value>	# 监听地址，可监听多个端口
-V		# 打印版本信息
-obfs4-distBias	# 开启obfs4

# <value>的格式
[scheme://][user:pass@host]:port[?param1=value1&param2=value2]
# scheme : 协议类型或传输类型，或二者的组合，或空
#		默认协议类型：-L参数是HTTP&SOCKS5，-F参数是HTTP
#		默认传输层类型：原始TCP类型
#		组合用法示例：http+tls, socks5+wss
```
