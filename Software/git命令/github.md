#### 免密登陆

[使用ssh免密登陆](https://help.github.com/en/articles/connecting-to-github-with-ssh)

```
ls -la ~/.ssh		# 查看本地是否已创建密钥
	# 我的目录里存在id_rsa.pub，表明本地已创建密钥

# 若已创建密钥，则将公钥复制到剪切板
sudo apt install xclip
xclip -sel clip < ~/.ssh/id_rsa.pub

# 登陆github，点右上角头像，选择"Settings"
# 点击“SSH and GPG keys”
# 点击"New SSH key","Title"任取，"Key"直接复制，然后点击"Add SSH key"

# 测试ssh连接
ssh -T git@github.com

# 将原仓库的https协议改为ssh
vim .git/config		# 修改url，将https://改为git@，将github.com/改为github.com:
```

