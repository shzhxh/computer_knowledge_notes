#### 简介

在MSDOS和Unix之间互相复制文件。

#### 语法

```bash
mcopy [-bspanvmQT] [-D clash_option] sourcefile targetfile
	# 从文件sourcefile复制到文件targetfile
mcopy [-bspanvmQT] [-D clash_option] sourcefile [ sourcefiles... ] targetdirectory
	# 从多个文件sourcefiles复制到目录targetdirectory
mcopy [-tnvm] MSDOSsourcefile
	# 将文件MSDOSsourcefile复制到当前文件夹下
	
# 注：如果文件名是 - ，则代表了标准输入或标准输出，取决于它在命令行里的位置。
```

#### 选项

```
i	# 指定要编辑的镜像
s	# 递归地复制
Q	# 当复制多个文件时，一旦失败则直接退出
```

