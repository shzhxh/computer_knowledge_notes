```
pip3 install pyserial	# 安装pyserial
```

```python
import serial	# 导入pyserial模块

# 方法
ser=serial.Serial("/dev/ttyUSB0", 115200, timeout=0.5)	# USB连接串口
ser.open()	# 打开端口
s = ser.read(10)	# 从端口读10个字节
data =ser.readline()	# 读一行
data = ser.readlines()	# 需设置超时时间
data = ser.xreadlines()	# 需设置超时时间
ser.isOpen()		# 判断端口是否打开
ser.write("hello")	# 向端口写数据
ser.close()		# 关闭端口

# 属性
print ser.name	# 设备名
print ser.port	# 端口名
ser.baudrate = 9600	# 设置波特率
ser.bytesize	# 字节大小，相关常量FIVE BITS、SIXBITS、SEVENBITS、EIGHTBITS(8位数据位)
ser.parity		# 校验位，N无校验，E偶校验，O奇校验
	# 相关常量PARITY_NONE(无校验), PARITY_EVEN(偶校验), PARITY_ODD(奇校验), PARITY_MARK, PARITY_SPACE
ser.stopbits	# 停止位，相关常量STOPBITS_ONE, STOPBITS_ONE_POINT_FIVE, STOPBITS_TWO
ser.timeout		# 读超时设置
ser.writeTimeout	# 写超时设置
ser.xonxoff		# 软件流控制
ser.rtscts		# 硬件流控制
ser.dsrdtr		# 硬件流控制
ser.interCharTimeout	# 字符间隔超时
```

#### serial.tools.miniterm

```
python -m serial.tools.miniterm -h	# 获取帮助
miniterm [options] [port] [baudrate]	# 连接到串口的程序，port即串口名
# 选项：端口设置
# 选项：数据处理
# 选项：热键
# 选项：诊断

# #########################
# 代码解析
# #########################
# 定义类ConsoleBase
# if os.name == 'nt'，定义类Console
# elif os.name == 'posix'，定义类Console
# else ，报错
# 定义类Transform,...
# 定义函数ask_for_port
# 定义类Miniterm
#   start()方法：开启了receiver_thread和transmitter_thread两个守护线程
#   reader()方法：在while循环里不停地读，直到产生异常才退出
#   writer()方法：在while循环里不停地写，直到接收到退出命令才退出
# 定义函数main
#   使用argparse解析参数
#   对参数的检查
#   while True打开端口
#   Miniterm实例化与设置
#   执行Miniterm的start()方法
#   退出miniterm的流程
# if __name__ == '__main__'，则执行main()
```

