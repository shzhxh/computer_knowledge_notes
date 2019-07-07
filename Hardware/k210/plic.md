#### 中断源

| 编号  | 外设                                                         | 描述     |
| ----- | ------------------------------------------------------------ | -------- |
| 0     | 无                                                           | 无中断   |
| 1~33  | SPI, I2S, I2C, UART, TIMER, RTC, WDT, APB, DVP, AI, FFT, DMA, UARTHS | 外设中断 |
| 34~65 | GPIOHS0~31                                                   | 外设中断 |

#### 中断寄存器在内存中的映射

| 地址                       | 寄存器名                                                     | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 0x0c000000<br> ~0x0c000FFF | priority[1024]                                               | 1023个优先级寄存器，对应所有外部中断源的优先级               |
| 0x0c001000<br>~0x0c001FFF  | u32[32]                                                      | pending array。一个bit对应一个中断源，所以只有前32个寄存器有效。 |
| 0x0c002000<br>~0x0c1FFFFF  | target[15872].enable[32]                                     | 核x使能。每个核都需要32个寄存器来对应所有的中断。            |
| 0x0c200000<br>~0x0FFFF004  | target[15872].priority_threshold<br>target[15872].claim_complete | 核x中断优先级的阈值，获取最高优先级的中断，完成中断的信号    |

#### 中断的操作

##### 初始化外部中断

plic_init

- target[core_id].enable[i]=0，当前核的使能位置0
- priority[i]=0，所有中断源的优先级置0
- target[core_id].priority_threshold=0，当前核的优先级阈值置0
- 清空所有核上中断源的pending bit(代码比较怪，无法理解)
- set_csr(mie, MIP_MEIP)，使能机器级外中断

##### 中断开启与关闭

plic_irq_enable

- 将中断号对应的使能寄存器赋值给current
- 将中断号对应的使能位写入current
- 将current写回到中断号对应的使能寄存器

plic_irq_disable

- 将中断号对应的使能寄存器赋值给current
- 清空current上中断号对应的使能位
- 将current写回到中断号对应的使能寄存器

##### 中断优先级的获取与设置

plic_set_priority

- 直接给对应的优先级寄存器赋值

plic_get_priority

- 直接返回对应的优先级寄存器的值

##### 中断请求的认领与完成

plic_irq_claim

- 直接返回target[core_id].claim_complete的值

plic_irq_complete

- 直接给target[core_id].claim_complete赋值

##### 用户回调函数的注册与取消注册

plic_irq_register

- 把函数指针赋值给对应的结构体plic_instance

plic_irq_unregister

- 把空指针赋值为对应的结构体plic_instance