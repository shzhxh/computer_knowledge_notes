#### 简介

管理jar文件

#### 语法

```
jar {cuxti}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] <files>
```

#### 操作参数

```
c 	// create a new jar archive
u	// update a jar archive
x	// extract files from a jar archive
t	// list the contants of a jar archive
i	// generate index information for a jar archive
```

#### 选项

```
-f 	//指定jar包的文件名
-v  	//生成详细的报造，并输出至标准设备
-m  	//指定manifest.mf文件.(manifest.mf文件中可以对jar包及其中的内容作一些一设置)
-0 	//产生jar包时不对其中的内容进行压缩处理
-M 	//不产生所有文件的清单文件(Manifest.mf)。这个参数与忽略掉-m参数的设置
-C 	//表示转到相应的目录下执行jar命令,相当于cd到那个目录，然后不带-C执行jar命令
```

#### 操作数

```

```



#### 例子

```
jar cf hello.jar hello	//创建jar包
jar cvf hello.jar hello	//创建jar包并显示打包过程
jar tvf hello.jar	//查看jar包的内容
jar xvf	hello.jar	//解压jar包
```