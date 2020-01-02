#### 简介

把多个object合成一个。

#### 用法(riscv64版本)

```
riscv64-unknown-elf-ld [options] file...
```

- **supported targets**

   elf64-littleriscv elf32-littleriscv elf64-little elf64-big elf32-little elf32-big plugin srec symbolsrec verilog tekhex binary ihex

- **supported emulations**

  elf64lriscv elf32lriscv

#### 选项

##### 通用选项

```
@<file>	# 从<file>里读取命令行选项
-A ARCH, --architecture ARCH	# 设置architecture
-b TARGET, --format TARGET  # 指定接下来输入文件的target
-l, --library=<namespec>	# 把namespec指定的库加到要链接的文件列表里
	# gcc编译或链接时的动态库搜索次序：
	# 1. -L选项指定的路径
	# 2. 环境变量LIBRARY_PATH指定的路径
	# 3. 默认目录：/lib:/usr/lib:/usr/lib64:/usr/local/lib
	# 程序执行时动态库的搜索次序:
	# 1. 编译时指定的搜索路径，由-Wl,rpath指定的路径
	# 2. 环境变量LD_LIBRARY_PATH指定的路径
	# 3. /etc/ld.so.conf.d/目录下的配置文件(ldconfig命令使生效)
	# 4. 默认目录：/lib:/usr/lib:/usr/lib64:/usr/local/lib
-L, --library-path=<searchdir>	# 把searchdir添加到一个路径列表里去，ld将在这个列表里搜索库和控制脚本。
-m EMULATION		# 设置emulation，可以用-V或--verbose列出所有的emulation。
-nostdlib			# 只在命令行明确指定的目录下搜索库
-v, --version		# 打印版本号。
-V					# 打印版本号，且列出所支持的emulations。

--dll-verbose, --verbose		# 除了-V的功能，还会打印出连接器将要使用的链接脚本。
-rpath=<dir>	# 把<dir>添加到运行时库的搜索路径里。
```

##### 支持的target

##### 支持的emulation

##### emulation定义的选项

