#### 简介

检测某个条件是否成立，检测对象可以是数值、字符或文件。

#### 语法

`test EXPRESSION`返回EXPRESSION的值，这个值只有两个结果：true或false。

##### 逻辑运算

- `! EXPRESSION`非运算
- `EXPRESSION -a EXPRESSION`且运算
- `EXPRESSION -o EXPRESSION`或运算

##### 字符串运算

- `-n STRING`字符串STRING长度非零则为真
- `STRING`等价于`-n STRING`
- `-z STRING`字符串STRING长度为零则为真
- `STRING1 = STRING2`两个字符串相等则为真
- `STRING1 != STRING2`两个字符串不相等则为真

##### 数值运算

- `INTEGER1 -eq INTEGER2`两个数值相等则为真
- `INTEGER1 -ne INTEGER2`两个数值不等则为真
- `INTEGER1 -ge INTEGER2`大于等于则为真
- `INTEGER1 -gt INTEGER2`大于则为真
- `INTEGER1 -le INTEGER2`小于等于则为真
- `INTEGER1 -lt INTEGER2`小于则为真

##### 文件运算

- ` FILE1 -ef FILE2`文件FILE1和FILE2有相同的设备和inode号
- ` FILE1 -nt FILE2`文件FILE1比文件FILE2修改时间要新
- ` FILE1 -ot FILE2`文件FILE1比文件FILE2修改时间要老
- `-b FILE`文件存在且为块设备
- `-c FILE`文件存在且为字符设备
- `-d FILE`文件存在且是一个目录
- `-e FILE`文件存在
- `-f FILE`文件存在且是普通文件
- `-g FILE`文件存在且设置了set-group-ID位
- `-G FILE`文件存在且属于有效的组
- `-h FILE`等价于`-L FILE`
- `-k FILE`文件存在且设置了sticky位
- `-L FILE`文件存在且是符号链接，等价于`-h FILE`
- `-O FILE`文件存在且其所有者为有效用户
- `-p FILE`文件存在且是有名管道
- `-r FILE`文件存在且可读
- `-s FILE`文件存在且其大小大于0
- `-S FILE`文件存在且是套接字
- `-t FD`文件描述符FD已经在terminal里打开
- `-u FILE`文件存在且set-user-ID位已设置
- `-w FILE`文件存在且可写则为真
- `-x FILE`文件存在且可执行则为真