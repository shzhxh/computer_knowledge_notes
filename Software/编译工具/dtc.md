#### 简介

 Device Tree Compiler，输入指定格式的设备树，输出可以启动内核的设备树。一般来说，输入的是人类可读的dts格式，而输出的是dtb格式或二进制格式。

#### 用法

```bash
dtc [options] <input file>
```

- `-I <input format>`指定输入文件的格式，可选的格式有：dts(device tree source text), dtb( device tree blob), fs( /proc/device-tree style directory).
- `-O <output format>`指定输出文件的格式，可选的格式有：dts, dtb, asm(assembler source).
- `-o <output file>`将结果输出到文件，而不是stdout。

#### DTS的语法

##### 概述

1. 由结点+结点属性组成。
2. 有且只有一个根结点，即“\”。
3. 根结点下又包含子结点，形成树形的结构。
4. 结点的属性放在`{}`里。可以是字符串"..."，整数<...>，十六进制数[...]，或为空。
5. 结点的名称是`node-name@unit-address`，其中`@unit-address`可以不选。

##### 属性

- compatible

  用于匹配驱动程序，优先级从左到右。

- interrupt

  指定中断的中断号，触发方法等。中断控制器结点中的`#inrerrupt-controller`控制此属性中值的个数。

- interrupt-parent

  指定它所依附的中断控制器。若没指定，则从双亲结点中继承。

- ranges

  描述该设备到双亲结点的地址映射关系。

- reg

  描述MMIO寄存器的位移和长度。这两个值的长度由双亲结点的`#address-cells`和`#size-cells`决定。

- 

##### chosen结点

描述由系统指定的运行时的参数，不描述硬件设备的信息。它的双亲结点必须是根结点。

##### aliases结点

定义别名。

##### memory结点

- device_type必须为"memory"