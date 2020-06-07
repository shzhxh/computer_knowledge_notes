```
groups [usernames]	# 列出username所在的组，如不指定username则列出当前用户所在的组
groups <--help | --version>
```

#### 一些特殊的组

##### uucp

通过`groups uucp`可见uucp就是uucp用户的属组。uucp用户在`/etc/passwd`中占一条记录，但不能登陆，它是一个伪用户。伪用户的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。

##### dialout

完整且直接地访问串口。这个组的成员可以重新配置modem，拨号到任何地方，等等。