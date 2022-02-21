#### 特殊目录：

​	.	//当前目录
​	..	//上一层目录
​	-	//上一个目录
​	~	//当前用户的用户目录
​	~a	//用户a的用户目录

#### 目录操作：

​	cd	// change directory
​	pwd	// print working directory
​	mkdir	// make directory
​	rmdir	// remove directory

#### 文件与目录管理：

​	ls, cp, rm, mv

#### 文件内容查阅：

​	cat, tac, nl, more, less, head, tail, od, file
change file timestamps:
​	touch

#### 修改系统隐藏权限：

​	lsattr, chattr

#### 默认权限：

​	umask

#### 文件特殊权限：

​	SUID
​	SGID
​	SBIT

#### 命令与文件搜寻：

​	which, type, whereis, locate, find

#### 重定向

##### 背景知识

重定向的意思很简单，就是捕获一个文件的输出，然后把它发送到另一个文件的输入。系统默认开启了三个文件，stdin(键盘)、stdout(屏幕)、stderr(输出到屏幕的错误信息)。stdin,stdout,stderr可以分别用0,1,2代表。如上默认打开的三个文件和其它打开的文件，之间都可以进行重定向。

##### 基本用法

1. `<`把文件重定向到stdin。

   `0<&-`或`<&-`关闭stdin

2. `[j]<>filename`打开文件filename并把文件描述符j赋给它。如不指定j则默认为stdin。

3. `>`把stdout重定向到一个文件，如文件不存在则会先创建文件，如文件存在则覆盖之。

   可以通过`fd>filename`的方式进行重定向，fd默认为1。

4. `>!`如果存在则覆盖

5. `&>`把stdout和stderr重定向到文件中

6. `>&`使用文件描述符进行重定向，形式为`M>&N`，M默认为1。

   `1>&-`或`>&-`关闭stdout

7. `>>`把stdout追加到文件中，如文件不存在则会先创建文件

8. `>>&`屏幕上的信息追加到文件中

9. `<<`将两个delimiter之间的document作为输入传递给command

   ```
   command << delimiter
   document
   delimiter
   ```

   

##### 用法示例

```shell
# 将屏幕输出的所有信息都放在文件test.log中
test.sh >& test.log
```

#### 参考文档

- [I/O Redirection](https://www.tldp.org/LDP/abs/html/io-redirection.html)