#### 寄存器

共包含7个32位寄存器

| 编号 | 名称   | 描述                                                         |
| ---- | ------ | ------------------------------------------------------------ |
| 0    | txdata | 传递数据。[0:7]数据，[8:30]为0，[31]满状态                   |
| 1    | rxdata | 接收数据。[0:7]数据，[8:30]为0，[31]空状态                   |
| 2    | txctrl | 传递控制。[0]tx使能，[1]0一个停止位,1二个停止位，[2:15]保留，[16:18]中断触发器的分频值，[19:31]保留 |
| 3    | rxctrl | 接收控制。[0]tx使能，[1:15]保留，[16:18]中断触发器的分频值，[19:31]保留 |
| 4    | ie     | 中断使能。[0]0禁止发送,1使能发送，[1]0禁止接收,1使能接收，[2:31]为0 |
| 5    | ip     | 中断pending。[0]小于txcnt，[1]大于rxcnt，[2:31]为0           |
| 6    | div    | 波特率divisor。[0:15]波特率divisor，[16:31]为0               |

#### 功能

- 初始化`uarths_init`

  设置波特率因子(div.div)为(CPU频率)/(115200-1)

  将tx和rx使能位置1(txctrl.txen、rxctrl.rxen)

  将tx和rx分频值置0(txctrl.txcnt、rxctrl.rxcnt)

  将ip.txwn和ip.rxwm置1

  将ie.txwm置0，ie.rxwm置1

- 传递和接收字符`uarths_[putchar | getchar]`

  传递字符：当txdata.full为1表示还有未传完的数据，只有当其为0时才可以写入数据。当txdata.full为0时，将要输入的字符串写入txdata.data。

  接收字符：当rxdata.empty为1时表示无数据可接收，当其为0时才表示rxdata.data为接收到的字符。

- 传递字符串`uarths_puts`

  使用`uarths_putchar`，以0作为终止条件。

- 设置中断回调`uarths_set_irq`

- 接收和发送数据`uarths_[receive | send]_data`

  与传递和接收字符类似，只是指定了数据的长度，这样就可以使用for语句了。

- 获取中断模式`uarths_get_interrupt_mode`

  中断模式有3种：发送(1)、接收(2)、发送和接收(3)。是从ip寄存器的rxwm和txwm获取的。

- 设置波特率和停止位`uarths_config`

  通过设置div.div为(CPU时钟频率)/(波特率-1)来使波特率生效。

  通过设置txctrl.nstop来设置停止位。

- 设置中断条件`uarths_set_interrupt_cnt`

  当为发送模式，则设置txctrl.txcnt来设置中断条件。

  当为接收模式，则设置rxctrl.rxcnt来设置中断条件。

  当为发送和接收模式，则设置txctrl.txcnt和rxctrl.rxcnt来设置中断条件。

#### 问题

1. 不理解中断模式为什么要从ip寄存器里获取，不应该是从ie寄存器里获取吗？