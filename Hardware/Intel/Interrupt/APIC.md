

#### APIC简介

（Advanced Programmable Interrupt Controller）是一个与8259A兼容的高级中断处理器。APIC分为两层：Local APIC和I/O APIC。

![APIC](_img/APIC.jpg)

- I/O APIC  I/O APIC是用来与外部设备通讯的，它安装在主板上，它完成了APIC最主要的功能：中断处理。 I/O APIC提供了两个模式，普通模式和8259A兼容模式。xv6在单核环境下，会选择使用8259A兼容模式，而在多处理器环境下，则会使用普通模式。
- Local APIC是APIC的顶层，每个核都有一个对应的Local APIC，LAPIC内置在CPU内部。它负责进行多处理器之间的中断传输，屏蔽中断，还提供了一个可编程的Timer。由于I/O APIC已经提供了中断处理的功能，Local APIC只是起辅助作用，可以屏蔽不用。

#### APIC的中断映射

| IRQ    | Using SERIRQ | Direct form Pin | Using PCI Message | Internal Modules              |
| ------ | ------------ | --------------- | ----------------- | ----------------------------- |
| 0      |              |                 |                   |                               |
| 1      |              |                 |                   |                               |
| 2      |              |                 |                   |                               |
| 3      |              |                 |                   |                               |
| 4      |              |                 |                   |                               |
| 5      |              |                 |                   |                               |
| 6      |              |                 |                   |                               |
| 7      |              |                 |                   |                               |
| 8      |              |                 |                   |                               |
| 9      |              |                 |                   |                               |
| 10     |              |                 |                   |                               |
| 11     |              |                 |                   |                               |
| 12     |              |                 |                   |                               |
| 13     |              |                 |                   |                               |
| 14     |              |                 |                   |                               |
| 15     |              |                 |                   |                               |
| 16~19  | PIRQA~PIRQD  | PIRQA~PIRQD     | Yes               | Internal devices are routable |
| 20～23 | N/A          | PIRQH           | Yes               |                               |

#### 控制方式

APIC通过修改该单元的寄存器来实现控制，但APIC的相关寄存器被映射到了物理地址。

- Local APIC默认映射到物理地址`0xFEE0 0000`
- IO APIC     默认映射到物理地址`0xFEC0 0000`

#### PRT表的格式

Programmable Redirection Table(24*64bit)，IOAPIC上有24个interrupt pin，每一个pin都对应一个RTE,所以针对每一个interrupt pin都可以单独设定它的mask，触发方式(level,edge trigger),中断管脚的极性，传送方式，传送状态，目的地，中断向量等。

| bit    | 意义                                                         |
| ------ | ------------------------------------------------------------ |
| 63：56 | Destination Field(Physical Mode:APIC ID, Logical Mode:一组CPU) |
| 55：17 | Reserved                                                     |
| 16     | Interrupt Mask(1:屏蔽中断，0:中断发给LAPIC)                  |
| 15     | Trigger Mode(1:Level，电平触发。0：Edge，边沿触发)           |
| 14     | Remote IRR,只读，只对Level触发有效。（1：LAPIC接收了该中断，0：LAPIC写EOI） |
| 13     | Interrupt Input Pin Polarity(INTPOL)(1:低电平，0：高电平)    |
| 12     | Delivery Status(0:IDEL,当前没有中断，1:Send Pending，IOAPIC已收到中断，但由于某种原因还没有发给LAPIC) |
| 11     | Destination Mode(0:Physical Mode, 1: Logical Mode)           |
| 10:8   | Delivery Mode(0:Fixed, 1:Lowest Priority, 2: SMI, 4: NMI, 5:INIT, 7:ExtINT ) |
| 7:0    | Interrupt Vector(0x10~0xFE, X86架构的前16个vector被系统预留) |

