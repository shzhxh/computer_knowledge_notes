#### 简介

在联网的主机之间加密复制文件。

#### 语法

`scp [options] [user@host1:]file1 [user@host2:]file2`

#### 选项

```
-P port	# 指定远程主机的端口
```

#### 问题解决

1 `Permission denied`

因为远程主机的目录权限不对，注意修改目录权限。
