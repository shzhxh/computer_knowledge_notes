```
fx [options] <cmd> [...]	# 运行Fuchsia开发命令。必须在包含源码的顶层目录执行。
fx help --full	# 完整的帮助
fx help <cmd>	# 关于<cmd>的详细帮助
```

选项

```
--dir <BUILD_DIR>
-d <DEVICE_NAME>
-i
-x
```

子命令：配置与编译

```
set <product>.<board>	# 设置要编译的产品<product>和硬件平台<board>
	# fx list-products查看可编译的产品
	# fx list-boards查看可编译的平台
```

