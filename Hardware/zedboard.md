### 功能测试

在ZedBoard自带的SD卡上就有一个简单的Linux镜像，用于展示Zedboard的基本功能。

#### 1. 连接跳线

确保跳线按如下形式连接：

| 标识符 | 连接形式 | 意义     |
| ------ | -------- | -------- |
| JP11   | GND      | 使用PLL  |
| JP10   | 3V3      | SD卡启动 |
| JP9    | 3V3      | SD卡启动 |
| JP8    | GND      | SD卡启动 |
| JP7    | GND      | JTAG级联 |
| JP6    | Shorted  | SD卡启动 |
| J18    | 1.8V     | 文档要求 |

#### 2. 启动系统

插入SD卡，开发板上Uart接口与电脑相连，连接电源，拨动开关，命令窗口中输入`sudo minicom -D /dev/ttyACM0`。看到提示符`zed-boot>`即表示成功进入U-BOOT。

输入`printenv`可以看到环境变量。

#### 3. 启动Linux

输入`run sdboot`开开始启动Linux，当看到提示符`zynq>`即表示成功进入linux。可以看到开发板上的显示屏显示出了图案。

```
read_sw		# 读取开关的状态
write_led 0xF0	# 使左边4个LED灯开，右边4个关
write_led 0xF	# 使左边4个关，右边4个关
write_led 0		# 使8个LED灯关闭
poweroff		# 关闭系统
```

### 运行Rocket

#### 1. 制作SD卡

```
git clone https://github.com/ucb-bar/fpga-zynq.git
cd fpga-zynq/zedboard
make fetch-images
# 电脑上插入SD卡，假定挂载在/mnt/abc
make load-sd SD=/mnt/abc
# SD卡再插入开发板
```

> boot.bin：system.bit + u-boot.elf + zynq_fsbl.elf
>
> uImage：ARM Linux
>
> uramdisk.image.gz：包含根文件系统的RAMDisk
>
> devicetree.dtb：设备树
>
> riscv/vmlinux：RISC-V Linux内核
>
> riscv/root.bin：包含根文件系统的RAMDisk(for RISC-V Linux)

#### 2. 启动Linux

在U-BOOT界面输入`run sdboot`启动Linux，用户名和密码均为root。

然而这有个问题，就是无法启动riscv核。我推测是riscv核根本就没有烧写到开发板里。

### 硬件结构

#### 跳线

| 标识符   | 描述                        | 默认设置              | 功能                                    |
| -------- | --------------------------- | --------------------- | --------------------------------------- |
| JP1      | 卖克风输入bias              | Open-无Electret卖克风 | 短接使能偏压                            |
| JP2      | Vbus 5V 使能                | Open-5V无连接         | 短接使能5V输出到OTG连接器               |
| JP3      | Vbus电容器设置              | Open-设备模式         | 短接用于Host模式，Open用于设备或OTG模式 |
| JP4      | CFGBVS选择                  | 无填充                |                                         |
| JP5      | PUDC选择                    | 无填充                |                                         |
| JP6      | MIO[0]                      | Short                 | SD卡启动                                |
| JP7      | Boot_Mode[3]/MIO[2]         | GND-级联JTAG          | JTAG模式。                              |
| JP8,9,10 | Boot_Mode[0,1,2]/MIO[3,4,5] | 110-SD卡              | 选择启动设备                            |
| JP11     | Boot_Mode[4]/MIO[6]         | GND-PLL用             | PLL选择。GND使用PS PLL，VCC绕过PS PLL.  |
| JP12     | XADC                        | Open                  |                                         |
| JP13     | JTAG PS-RST                 | Open                  | 短接将JTAG PROG-RST连接到PS Reset.      |
| JP18     | Vadj选择                    | 1.8V                  |                                         |

#### 启动模式

|                | MIO6/JP11 | MIO5/JP10 | MIO4/JP9 | MIO3/JP8 | MIO2/JP7 |
| -------------- | --------- | --------- | -------- | -------- | -------- |
| JTAG模式(级联) |           |           |          |          | 0        |
| JTAG模式(独立) |           |           |          |          | 1        |
| 启动设备(JTAG) |           | 0         | 0        | 0        |          |
| 启动设备(QSPI) |           | 1         | 0        | 0        |          |
| 启动设备(SD卡) |           | 1         | 1        | 0        |          |
| PLL(使用)      | 0         |           |          |          |          |
| PLL(绕过)      | 1         |           |          |          |          |


