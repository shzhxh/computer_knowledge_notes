```
keytool [commands]	# 管理keystore
	# keystore是一个数据库，它包含了密钥，X.509证书链，可信证书。
```

#### 命令

```
# 一、创建keystore或向keystore中添加数据
-gencert
-genkeypair	# 生成密钥对(公钥和私钥)。
	-alias <alias>
	-keyalg <keyalg>	# 指定要生成的密钥对的算法
	-keysize <keysize>	# 指定要生成的密钥的大小
	-validity <valDays>	# 指定证书的有效期(如使用了-startdate选项则从它指定的日期开始，如没使用-startdate选项则从当前日期开始)
-genseckey
-importcert
-importpassword


# 二、展示数据
-list
-printcert
-printcertreq
-printcrl

# 三、管理keystore
-storepasswd
-keypasswd
-delete
-changealias

# 四、其它未分类
-importkeystore	# 从其它的keystore注入内容
-certreq	# 生成证书请求
-exportcert	# 输出数据
-help	# 获取帮助
```

#### 通用选项

```
-v		# 展示详细信息
-storetype <storetype>
-keystore <keystore>	# 给定keystore的位置
```



#### 示例

```
# 生成keystore
keytool -genkeypair -keystore key.store -alias my_alias -keyalg RSA -keysize 4096 -validity 10000
```

