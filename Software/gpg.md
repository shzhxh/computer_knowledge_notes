#### 作用

加密和签名工具。

#### 背景知识

加密分对称加密和非对称加密。

#### 语法

```
gpg [选项] 命令 [参数]
```

#### 命令

```
-c, --symmetric 对称加密，默认使用AES-128，可用--cipher-algo选项选项加密方式。
-e, --encrypt  使用公钥进行非对称加密。
-d, --decrypt  解密文件，默认输出到SDTOUT。
--gen-key, --generate-key  生成非对称密钥。
-k, --list-keys, --list-public-keys  列出指定的密钥。
-k, --list-sectet-keys   列出指定的密钥。
--export  导出钥匙，默认输出到stdout。
--export-secret-keys, --export-secret-subkeys 导出私钥，默认输出到STDOUT。
--import, --fast-import  导入钥匙
-s, --sign 对文件签名，生成的是二进制文件。
--clear-sign, --clearsign  生成可读的签名，生成的是文本文件。
--verify  对签名进行验证
```



#### 选项

```
-o, --output   指定要输出的文件。
-a, --armor   创建ASCII护甲／/输出，默认是创建二进制OpenPGP格式。
-r, --recipient  使用用户名加密。
```

#### 例子

```
对称加密解密
gpg -c myfile	# 对myfile进行对称加密，会生成加密后的文件myfile.gpg
gpg -o myfile_e -d myfile.gpp	# 解密myfile.gpg到文件myfile_e里

非对称加密解密
gpg --gen-key	# 生成钥匙对
gpg --list-keys	# 查看已有钥匙
gpg -e -r user myfile	# 使用用户名user的公钥对myfile加密
gpg -o myfile_e -d myfile.gpg	# 解密myfile.gpg到文件myfile_e里

导入导出钥匙
gpg -o mykey --export keyID  # 使用公钥keyID导出公钥
gpg -o mykey --export-secret-keys keyID  # 使用私钥keyID导出私钥
gpg --import mykey  # 导入钥匙

签名与验证
gpg -s myfile	# 对myfile签名
gpg --verify  myfile  # 对myfile验签
```
