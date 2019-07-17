#### 简介

删除用户与相关文件

#### 语法

`userdel [options] LOGIN`

#### 选项

```
-f, --force	# 强制删除与用户相关的所有内容
-h, --help
-r, --remove	# 删除用户目录下的所有文件
-R, --root CHROOT_DIR
-z, --selinux-user
```

#### 配置

`/etc/login.defs`里的配置参数会改变此工具的行为。