#### USB HID工作原理

HID(Human Interfafe Device)即USB人机接口设备，用于为满足USB标准的设备进行高度通用的支持。

USB HID设备向主机提供一个Input Report流，描述自已的状态。(对于USB键盘来说，就是每个键的状态)。这个Input Report的是二进制数据，它的结构可能是设备相关的，也可能是任意的。

既然输入的二进制数据结构是不固定的，主机怎么能理解它们呢？靠的是设备向主机发送Report Descriptor，这是由HID规范定义的。

Report Descriptor定义了Input Report包含了哪些字段，以及这些字段的大小、偏移、含义。Report Descriptor语言能力有限，但也足够覆盖HID设备的绝大多数用例了。

- 可以用一个给定范围的整数表示一个输入。例如，键盘就是用一个位来表示一个键的。
- 可以表示任意多个输入。
- 可以用整数数组来表示输入。
- Report Descriptor里的输入必须与"usage code"相关联，而"usage code"定义在USB HID规范里。例如键盘里的每个键都对应了"usage code"。

除了Input Report，还有Output Report，是由主机发给设备的流。这两者以完全类似的方式工作，且由相同的Report Descriptor来描述。

#### USB keyboard工作原理

由USB HID的工作原理可知，完全可以用一个106位的位域来表示一个106键的USB键盘。但为什么又有“USB仅限于6键翻转”的流言呢？

由于HID Report Descriptor的格式比较复杂，为了简化BIOS程序员的工作，在HID规范里包含了一个"boot keyboard"的概念。

一个满足"boot protocol"的键盘需要特定格式的Input Report。这样，BIOS就可以不看Report Descriptor，直接使用Input Report了。

“boot protocol”格式是在一个字节里用1个位代表8个modifier key，1个位保留，6个位代表6个scancode。

所有键盘都实现了"boot protocol"，这就是6键限制的由来。但"boot protocol"并非唯一选择，也可以选择键盘默认的报告格式。

#### 参考文档

[Myths about USB NKRO and how USB HID works](https://www.devever.net/~hl/usbnkro#:~:text=A%20keyboard%20complying%20with%20the%20%E2%80%9Cboot%20protocol%E2%80%9D%20is,have%20to%20look%20at%20the%20corresponding%20Report%20Descriptor.)

