#### 简介

运行一个System V 初始化脚本。

`service`会尽量在一个可预测的环境中运行System V初始化脚本或systemd单元，(它会)移除大多数环境变量且将当前工作目录设置为根目录。

#### 语法

```
service <SCRIPT> <COMMAND> [OPTIONS]
	# SCRIPT是System V初始化脚本，或systemd单元的名称。若重名，则systemd单元的优先级较高。
	# systemd单元在/{lib,run,ect}/systemd/system目录下。
	# Ssytem V初始化脚本在/etc/init.d目录下。
	# COMMAND由SCRIPT提供支持。至少应实现的命令是start和stop。
service --status-all	# 运行所有初始化脚本(以字母序)的status命令
service --help | -h | --version
```

#### 环境变量

初始化脚本仅接受如下环境变量

```
LANG,  LANGUAGE, LC_CTYPE, LC_NUMERIC, LC_TIME, LC_COLLATE, LC_MONETARY, LC_MESSAGES, LC_PAPER,LC_NAME, LC_ADDRESS, LC_TELEPHONE, LC_MEASUREMENT, LC_IDENTIFICATION, LC_ALL, TERM, PATH
```

