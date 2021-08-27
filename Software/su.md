#### 简介
变为其他用户。

#### 语法

`su [options] [username]`

#### 选项

```
-c, --command COMMAND	# 指定shell要执行的命令
-, -l, --login		# 提供与用户直接登陆相似的环境，-必须用在username之前，-l和--login则无此限制
-s, --shell SHELL	# 指定要使用的shell
-m, -p, --preserve-environment	# 保留当前环境，但不包括$PATH和$IFS
```



#### 配置

/etc/login.defs里的如下变量将改变此命令的行为：

```
CONSOLE_GROUPS (string)
DEFAULT_HOME (boolean)
ENV_PATH (string)
ENV_SUPATH (string)
SULOG_FILE (string)
SU_NAME (string)
SYSLOG_SU_ENAB (boolean)
```



#### 文件

```
/etc/passwd
/etc/shadow
/etc/login.defs
```



#### 返回值

```
0	# 成功
1	#　
126	#
127	# 
>128	#
255	# 
```

#### 示例

```
su -c whoami szx	# 以用户szx运行whoami命令
```

