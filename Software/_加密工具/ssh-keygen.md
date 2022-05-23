```shell
ssh-keygen [-q] [-b <bits>] [-t <key-type>] [-N <new_passphrase>] [-C <comment>] [-f <output_keyfile>]	# 生成密钥
```

选项

```shell
-f <filename>	# 指定密钥文件的文件名
-t <dsa|ecdsa|ecdsa-sk|ed25519|ed25519-sk|rsa>
		# 指定密钥的类型。在没有参数的情况下，默认rsa。
```

