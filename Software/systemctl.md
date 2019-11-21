#### 简介

控制systemd系统和服务管理器。请先使用`man systemd`命令了解systemd。

#### 语法

```bash
systemctl [options] command [unit]
```

#### 选项

#### 命令

##### Unit命令

```
list-units	# 列出systemd当前在内存中的units
start	# 开启某项服务
restart	# 重启某项服务
stop	# 关闭某项服务
status	# 检查服务状态
```



##### Unit文件命令

```
enable	# 使某项服务自动启动。其作法是加载一组符号链接，然后重载系统管理器的配置。
disable	# 使某项服务不自动启动。其作法是删除相应的符号链接，从而使enable或link命令失效。
```



##### 机器命令

##### 作业命令

##### 环境命令

##### 管理生命周期

##### 系统命令

##### 参数的语法

#### 实例

```bash
systemctl start libvirtd	# 开启libvirtd服务
```

