```bash
cryptsetup <options> <action> <args>
```

#### 基本的动作

##### open

```
open <dev> <name> --type <dev_type>	# 基于设备dev打开name
	# dev_type可能的值：plain, luks(默认), luks1, luks2, loopaes, tcrypt
	# open的别名：
	#   create : open --type plain
	#   plainOpen : open --type plain
	#   luksOpen : open --type luks
	#   loopaesOpen : open --type loopaes
	#   tcryptOpen : open --type tcrypt
	#   bitlkOpen : open --type bitlk
```



##### close

```
close <name>	# 删除映射的name，并消除内核空间里的key。
	# close的别名：remove, plainClose, luksClose, loopaesClose, tcryptClose
```



##### status

##### resize

```
resize [options] <name>	# 修改<name>的大小，其中<name>是一个活动的映射。
```



##### refresh

##### reencrypt

#### PLAIN模式

#### LUKS扩展

##### luksFormat

```
luksFormat <dev> [key-file]	# 初始化LUKS分区并设置密码。
```

##### open

```
open --type luks <dev> <name>
luksOpen <dev> <name>	# 旧语法
```



#### loop-AES扩展

#### TCRYPT扩展

#### BITLK扩展

#### 其它动作

#### 选项

```
--verbose, -v	# 命令执行的时候打印详细信息
--verify-passphrase, -y	# 要求提供密码
```



#### 示例

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

#### 配置

```
# 配置文件是/etc/crypttab，详见man crypttab
```

