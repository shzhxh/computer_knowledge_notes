#### 简介

管理核间中断。

#### 寄存器

| 地址                      | 寄存器名       | 描述                                                         |
| ------------------------- | -------------- | ------------------------------------------------------------ |
| 0x02000000~<br>0x02003FFF | msip[4095]     | 4094个核的msip寄存器(机器级软件中断待处理寄存器)，每个msip寄存器只有第一位(MSIP位)是有效的，用于记录是否发生了机器级软件中断 |
| 0x02004000~<br>0x0200BFF0 | mtimecmp[4095] | 4094个核的mtimecmp寄存器。当mtime中的值大于等于某个核的mtimecmp中的值时，那个核上就会发生计时器中断。计时器中断会影响对应核mip寄存器的MTIP位。 |
| 0x0200BFF8                | mtime          | mtime寄存器。是一个64位寄存器，记录了记数器的实时值。        |

#### 操作

##### 初始化clint计时器

clint_timer_init

- clear_csr(mie, MIP_MTIP)，禁止机器级计时器中断

##### 从clint计时器获取时间

clint_get_time

- 返回寄存器mtime的值

##### clint计时器的开启或关闭

clint_timer_start

- 设置计时器的时间间隔，以及是否单发射
- 设置当前核的mtimecmp的值，即mtime的值加上一个cycles
- 在mstatus寄存器里使能机器级中断
- 在mie寄存器里使能计时器中断

clint_timer_stop

- clear_csr(mie, MIP_MTIP)，禁止机器级计时器中断

##### 计时器间隔的获取与设置

clint_timer_get_interval

- 直接返回当前核下时间间隔的值

clint_timer_set_interval

- 直接设置当前核下时间间隔的值
- 通过时间间隔计算出cycles的值

##### 计时器单发射的获取与设置

clint_timer_get_single_shot

- 直接获取当前核single_shot的值

clint_timer_set_single_shot

- 直接设置当前核single_shot的值

##### 计时器超时的时候用户回调函数的注册与取消注册

clint_timer_register

- 直接把函数指针赋值给结构体clint_timer_instance

clint_timer_unregister

- 直接把空指针赋值给clint_timer_register

##### 核间中间的初始化、使能、取消使能

clint_ipi_init

- 取消机器级软件中断

clint_ipi_enable

- 在mstatus寄存器里使能机器级中断
- 在mie寄存器里使能机器级软中断

clint_ipi_disable

- 在mie寄存器里关闭机器级软中断

##### 核间中断的发送与清除

clint_ipi_send

- 将对应核的msip寄存器的MSIP位置1

clint_ipi_clear

- 将对应核的msip寄存器的MSIP位置0

##### 核间中断回调函数的注册与取消注册

clint_ipi_register

- 直接把函数指针赋值给结构体clint_ipi_instance

clint_ipi_unregister

- 直接把空指针赋值给clint_ipi_register