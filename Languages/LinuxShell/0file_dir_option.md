#### 特殊目录：

​	.	//当前目录
	..	//上一层目录
	-	//上一个目录
	~	//当前用户的用户目录
	~a	//用户a的用户目录

#### 目录操作：

​	cd	// change directory
	pwd	// print working directory
	mkdir	// make directory
	rmdir	// remove directory

#### 文件与目录管理：

​	ls, cp, rm, mv

#### 文件内容查阅：

​	cat, tac, nl, more, less, head, tail, od, file
change file timestamps:
	touch

#### 修改系统隐藏权限：

​	lsattr, chattr

#### 默认权限：

​	umask

#### 文件特殊权限：

​	SUID
	SGID
	SBIT

#### 命令与文件搜寻：

​	which, type, whereis, locate, find

#### 重定向

##### 背景知识

0,1,2分别代表stdin,stdout,stderr

##### 基本用法

1. `<`重定向输入
2. `>`重定向输出
3. `>!`如果存在则覆盖
4. `>&`屏幕输出的所有信息都放在指定文件中
5. `>>`追加到文件中
6. `>>&`屏幕上的信息追加到文件中

##### 用法示例

```shell
# 将屏幕输出的所有信息都放在文件test.log中
test.sh >& test.log
```

