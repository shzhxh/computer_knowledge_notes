kendryte UART ISP工具，往k210里刷入数据。

```
kflash [options] <firmware>
```

#### 选项

```
-p, --port <PORT>	# COM端口
-f, --flash <FLASH>	# SPI Flash类型(0:SPI3, 1:SPI0)
-b, --baudrate <BAUDRATE>	# UART波特率，用于上载固件
-l, --bootloader <BOOTLOADER>	# bootloader的路径
-k, --key <KEY>		# 16进制的AES密钥，如果需要加密固件的话
--verbose			# 输出更多信息
-t, --terminal		# 结束后开启一个终端(Python miniterm)
-n, --noansi		# 不使用ANSI配色，建议在Windows CMD里使用
-s, --sram			# 把固件下载到SRAM里然后启动
-B, --Board	<BOARD>	# 选择一个开发板，可用选项有kd233,dan,bit,bit_mic,goE,goD,maixduino,trainer
-S, --Slow			# 缓慢下载的模式
```

