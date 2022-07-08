```
gpasswd [option] <group>	# 管理/etc/group和/etc/gshadow

# 选项：
# 除了-A和-M，只能使用一个选项
-a, --add <user>	# 把<user>加到<group>
-d, --delete <user>	# 把<user>从<group>里删除
-Q, --root <CHROOT_DIR>	# 使用<CHROOT_DIR>目录里的配置文件修改<CHOOT_DIR>目录
-r, --remove-password	# 删除组的密码
-R, --restrict	# 只能使用newgrp命令，且有密码的用户才可以加入组里
-A, --administrators <user,...>	# 设置管理员的列表
-M, --members <user,...>	# 设置组里的成员
```

