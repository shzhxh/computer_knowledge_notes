### 简介

对应代码在[shzhxh/ucore_os_lab](https://gitee.com/shzhxh/ucore_os_lab.git)上，分支**riscv32-priv-1.10**来自于[chyyuu/ucore_os_lab](https://github.com/chyyuu/ucore_os_lab)的**riscv32-priv-1.10**分支，分支**riscv64-priv-1.10**是到riscv64的移植，分支**paper**也是到riscv64的移植版，目的是可以跑在FU540-C000上。

### RV64与RV32的差异

#### 1. 寄存器
寄存器的名称和数量均一致，只是长度不同。RV64I是64位的，RV32I是32位的。
#### 2. 指令集
RV32I的指令RV64I均具备，且它们的编码是一致的，均为32位，不同点是RV32I指令操作的数据对象是32位的，而RV64I指令操作的数据对象是64位的。RV64I增设的指令均是为了操作32位数据而增加的。需要注意的是，伪指令rdcycle,rdtime,rdinstret分别读取的是相应寄存器的全部64位(cycle,time,instret计数器)，所以rdcycleh, rdtimeh, rdinstreth在RV64I里就不需要了，是非法的。
#### 3. 内存管理

RV32的内存管理规约是Sv32，RV64的内存管理规约是Sv39或Sv48。它们都是采用多级页表映射的方式实现从虚拟内存地址到物理内存地址的转换的。所不同的是Sv32是二级映射，Sv39是三级映射，Sv48是四级映射。Sv32的每个页表项占用4个字节，而Sv39和Sv48的每个页表项占用8个字节。Sv32是32位虚拟地址映射到34位物理地址，Sv39是39位的虚拟地址映射到56位的物理地址，Sv48是48位的虚拟地址映射到56位的物理地址。

#### 4. 其它

地址长度不同，elf格式不同，BBL是经过裁减的，RISCV的函数参数在寄存器而X86在内存，栈里每个单元占的字节数不同。

#### 5. 总结

由于Ucore是使用C语言开发的，使用的接口也是SBI，所以移植的时候基本上可以不用考虑64位和32位指令集之间的差异。主要需要考虑的是寄存器里相应位的意义的改变，以及内存管理规约的不同。注：虽然不需要太关注指令集和寄存器的差异，但需要熟悉指令集和寄存器。

### 移植过程

#### lab1 中断子系统

指针类型不应定义为`int`或`uint`类型，而应定义为`intptr_t`或`uintptr_t`类型。同时在defs.h里对指针类型做如下更改：

```c
#if __riscv_xlen == 32
typedef int32_t intptr_t;
typedef uint32_t uintptr_t;
#elif __riscv_xlen == 64
typedef int64_t intptr_t;
typedef uint64_t uintptr_t;
#endif
```

修改Makefile以使用rsicv64工具链，运行`make qemu`即可在qemu中运行。



### 参考资料

1. [sifive all aboard系列](https://www.sifive.com/blog/)
2. riscv spec 2.2
3. riscv privileged 1.10
4. ucore_os_docs