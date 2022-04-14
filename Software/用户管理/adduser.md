#### 描述

```bash
adduser [options] <user>	# 添加一般用户(可以登陆并管理资源的用户)
adduser --system [options] <user> # 添加系统用户(系统用户仅用于运行服务程序)
```

#### 选项

```
--system	# 创建系统用户或系统组。
```

#### 错误解决

1. This account is currently not available.

   原因：创建了一个系统用户，默认shell是/sbin/nologin。

   解决方法：编译/etc/passwd，把shell改为/bin/bash。
