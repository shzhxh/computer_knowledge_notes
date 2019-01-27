RISC-V ISA手册

本手册分为两卷，上卷讲用户级指令，下卷讲特权级指令。

### 一些待解决的问题

#### 1. riscv里的ecall指令与x86里的int指令的异同。

ecall是用来触发异常的，而int是用来触发中断的，感觉它们用的地方也很相似，那么它们之间有什么异同呢？

#### 2. 为什么即有LW指令也有LWU指令，而只有SW指令没有SWU指令呢？

如果无符号数的宽度小于寄存器的宽度，执行普通的LW指令就会有问题，因为它会进行符号扩展，这就需要LWU指令把高位全都填充为0。而把数保存到寄存器中就不存在这样的问题，因为数的大小和占用的内存大小是完全一致的。

#### 3. 为什么有算术右移而没有算术左移呢？



### RISC-V 手册上卷(riscv-spec-v2.2)

#### 1. 介绍

   设计目标。不使用商业ISA的原因。不使用OpenRISC的原因。

   1. RISC-V ISA概述

      共有3种类型的指令：基本指令，标准扩展指令和特殊扩展指令。基本指令被命名为I，是不可更改，必须被包含的指令，它们都是整数指令。标准扩展指令包括M(扩展乘除指令)，A(扩展原子指令)，F(单精度浮点指令扩展)，D(双精度浮点指令扩展)，G(IMAFD五种指令的总和，提供了完整的通用标量指令集，是默认工具链支持的指令集)。特殊扩展指令只用于特定领域，不适用于全部通用领域。

   2. 指令长度编码

      基本指令固定是32位的，然而扩展指令可以是变长的，但必须是16的倍数。当低2位不是11时指令长度为16位，当低2位是11时指令长度为32位，当低5位为11111时指令长48位，当低6位为111111时指令长64位，当低7位全1则要计算[14:12]位的数才能得出指令长度。

      基本指令集是小端系统，但扩展指令集即可做成小端也可做成大端。

   3. 异常、陷阱和中断

      异常是指令错误引起的，陷阱是线程自主调用的，中断是外部事件引起的。

#### 2. RV32I指令集

   RV32I是基本指令集，它被设计足以支持一个现代的操作系统，而且除了A扩展外它基本上能模拟所有的其它扩展。RV32I共计47条指令。

#####   1. 程序员模型

      用户可见的寄存器有3种：1个恒为0的x0，31个通用寄存器x1-x31，pc。

#####   2. 基本指令格式

      在基本ISA里，所有指令都是32位且必须4字节对齐。共有四种基本格式：R-type是寄存器-寄存器操作，I-type是寄存器-立即数操作，S-type，U-type。
    
      立即数字段里放的是立即数的位置，而不是它本身。
    
      R-type: **funct7**[31:25] **rs2**[24:20] **rs1**[19:15] **funct3**[14:12] **rd**[11:7] **opcode**[6:0]
    
      I-type: **imm[11:0]**[31:20] **rs1**[19:15] **funct3**[14:12] **rd**[11:7] **opcode**[6:0]
    
      S-type: **imm[11:5]**[31:25] **rs2**[24:20] **rs1**[19:15] **funct3**[14:12] **imm[4:0]**[11:7] **opcode**[6:0]
    
      U-type: **imm[31:12]**[31:12] **rd**[11:7] **opcode**[6:0]

#####   3. 立即数编码的变体

      B是S的变体，J是U的变体
    
      B-type: **imm[12]imm[10:5]**[31:25] **rs2**[24:20] **rs1**[19:15] **funct3**[14:12] **imm[4:1]imm[11]**[11:7] **opcode**[6:0]
    
      J-type:**imm[20]imm[10:1]imm[11]imm[19:12]**[31:12] **rd**[11:7] **opcode**[6:0]

#####   4. 整数计算指令(共计22条指令)

      | imm+rs1+funct3+rd+opcode                 | funct3 | opcode | 意义                                       |
      | ---------------------------------------- | ------ | ------ | ---------------------------------------- |
      | ADDI rd, rs1, imm                        | ADDI   | OP-IMM | rd = rs1 + imm                           |
      | SLTI rd, rs1, imm (set less than immediate) | SLTI   | OP-IMM | rd = (rs1<imm) ? 1 : 0(用于有符号数)           |
      | SLTIU rd, rs1, imm                       | SLTIU  | OP-IMM | rd = (rs1<imm) ? 1 : 0(用于无符号数);**注sltiu rd,rs,1的意思是rd = (rs==0)?1:0;** |
      | ANDI rd, rs1, imm                        | ANDI   | OP-IMM | rd = rs1 & imm                           |
      | ORI rd, rs1, imm                         | ORI    | OP-IMM | rd = rs1 \| imm                          |
      | XORI rd, rs1, imm                        | XORI   | OP-IMM | rd = rs1 ^ imm                           |
      | SLLI rd, rs1, imm                        | SLLI   | OP-IMM | rd = rs1 << imm[4:0]                     |
      | SRLI rd, rs1, imm                        | SRLI   | OP-IMM | rd = (unsigned) rs1 >> imm[4:0]          |
      | SRAI rd, rs1, imm                        | SRAI   | OP-IMM | rd = (signed) rs1 >> imm[4:0]            |
    
      | imm+rd+opcode                            | opcode | 意义                    |
      | ---------------------------------------- | ------ | --------------------- |
      | LUI rd, imm (load upper immediate)       | LUI    | rd = (imm << 12)      |
      | AUIPC rd, imm (add upper immediate to pc) | AUIPC  | rd = (imm << 12) + PC |
    
      | funct7+rs2+rs1+funct3+rd+opcode | funct7  | funct3 | opcode | 意义                                       |
      | ------------------------------- | ------- | ------ | ------ | ---------------------------------------- |
      | ADD rd, rs1, rs2                | 0000000 | ADD    | OP     | rd = rs1 + rs2                           |
      | SLT rd, rs1, rs2                | 0000000 | STL    | OP     | rd = (rs1<rs2) ? 1 : 0 (用于有符号数)          |
      | SLTU rd, rs1, rs2               | 0000000 | STLU   | OP     | rd = (rs1<rs2) ? 1 : 0 (用于无符号数);**注sltu rd, x0, rs的意思是rd = (rs != 0) ? 1 : 0;** |
      | AND rd, rs1, rs2                | 0000000 | AND    | OP     | rd = rs1 & rs2                           |
      | OR rd, rs1, rs2                 | 0000000 | OR     | OP     | rd = rs1 \| rs2                          |
      | XOR rd, rs1, rs2                | 0000000 | XOR    | OP     | rd = rs1 ^ rs2                           |
      | SLL rd, rs1, rs2                | 0000000 | SLL    | OP     | rd = rs1 << (rs2 & 0x1F )                |
      | SRL rd, rs1, rs2                | 0000000 | SRL    | OP     | rd = (unsigned) rs1 >> (rs2 & 0x1F)      |
      | SUB rd, rs1, rs2                | 0100000 | SUB    | OP     | rd = rs1 - rs2                           |
      | SRA rd, rs1, rs2                | 0100000 | SRA    | OP     | rd = (signed) rs1 >> (rs2 & 0x1F)        |
    
      NOP指令的编码：ADDI x0, x0, 0

#####   5. 控制转移指令(8条指令)

- 无条件跳转(U-type or J-type)
| 指令                                      | 编码                              | 意义                          |
| ----------------------------------------- | --------------------------------- | ----------------------------- |
| JAL rd, imm(jump and link)                | imm+rd+JAL(opcode)                | rd = pc + 4; pc += imm;       |
| JALR rd, rs1, imm(jump and link register) | imm+rs1+0(funct3)+rd+JALR(opcode) | rd = pc + 4; pc += rs1 + imm; |

- 条件分支(S-type or B-type)
| imm+rs2+rs1+funct3+imm+opcode | funct3   | opcode | 意义           |
| ----------------------------- | -------- | ------ | ------------ |
| BEQ rs1,rs2,offset            | BEQ      | BRANCH | if(rs1==rs2) |
| BNE rs1,rs2,offset            | BNE      | BRANCH | if(rs1!=rs2) |
| BLT/BLTU rs1, rs2, offset     | BLT/BLTU | BRANCH | if(rs1<rs2)  |
| BGE/BGEU rs1, rs2, offset     | BGE/BGEU | BRANCH | if(rs1>=rs2) |

#####   6. LOAD和STORE指令(8条指令)

      | imm+rs1+funct3+rd+opcode | funct3 | opcode | 意义            |
      | ------------------------ | ------ | ------ | --------------- |
      | LW rd, imm(rs1)          | 宽度   | LOAD   | rd=mem(rs1+imm) |
      | LH rd, imm(rs1)          | 宽度   | LOAD   | rd=mem(rs1+imm) |
      | LHU rd, imm(rs1)         | 宽度   | LOAD   | rd=mem(rs1+imm) |
      | LB rd, imm(rs1)          | 宽度   | LOAD   | rd=mem(rs1+imm) |
      | LBU rd, imm(rs1)         | 宽度   | LOAD   | rd=mem(rs1+imm) |
    
      | imm+rs2+rs1+funct3+imm+opcode | funct3 | opcode | 意义               |
      | ----------------------------- | ------ | ------ | ---------------- |
      | SW(32位)                       | 宽度     | STORE  | mem(rs1+imm)=rs2 |
      | SH(16位)                       | 宽度     | STORE  | mem(rs1+imm)=rs2 |
      | SB(8位)                        | 宽度     | STORE  | mem(rs1+imm)=rs2 |
    
      ​

#####   7. 内存模型(2条指令)

本节已 **过时**，新的内存模型正在修订中。
| imm[11:0]                         | rs1     | funct3  | rd      | opcode   | 意义        |
| --------------------------------- | ------- | ------- | ------- | -------- | --------- |
| imm[11:8]保留，imm[7:4]前续，imm[3:0]后续 | 保留，应置为0 | FENCE   | 保留，应置为0 | MISC-MEM | 顺序化IORW访问 |
| imm[11:0]保留，应置为0                  | 保留，应置为0 | FENCE.I | 保留，应置为0 | MISC-MEM | 同步指令和数据流  |


​      

#####   8. 控制和状态寄存器指令(6条指令)
- CSR指令

| csr+rs1+funct3+rd+opcode | rs1       | funct3 | opcode | 意义                         |
| ------------------------ | --------- | ------ | ------ | ---------------------------- |
| csrrw rd, csr, rs1       | source    | CSRRW  | SYSTEM | tmp=rs1; rd=csr; csr=tmp     |
| csrrs rd, csr, rs1       | source    | CSRRS  | SYSTEM | tmp=rs1; rd=csr; csr\|=tmp   |
| csrrc rd, csr, rs1       | source    | CSRRC  | SYSTEM | tmp=rs1; rd=csr; csr &= !tmp |
| csrrwi rd, csr, rs1      | uimm[4:0] | CSRRWI | SYSTEM | rd=csr; csr=uimm[4:0]        |
| csrrsi rd, csr, rs1      | uimm[4:0] | CSRRSI | SYSTEM | rd=csr; csr\|=uimm[4:0]      |
| csrrci rd, csr, rs1      | uimm[4:0] | CSRRCI | SYSTEM | rd=csr; csr\|=uimm[4:0]      |

- 计时器和计数器(伪指令)
  
| csr+rs1+funct3+rd+opcode | csr           | rs1  | funct3 | opcode | 意义             |
| ------------------------ | ------------- | ---- | ------ | ------ | -------------- |
| rdcycle[h] rd            | RDCYCLE[H]    | 0    | CSRRS  | SYSTEM | 读取cycle CSR    |
| rdtime[h] rd             | RDTIME[H]     | 0    | CSRRS  | SYSTEM | 读取time CSR     |
| rdinstrent[h] rd         | RDINSTRENT[H] | 0    | CSRRS  | SYSTEM | 读取instrent CSR |

##### 9. 环境调用和断点(2条指令)

      | funct12 | rs1  | funct3 | rd   | opcode | 意义           |
      | ------- | ---- | ------ | ---- | ------ | ------------ |
      | ECALL   | 0    | PRIV   | 0    | SYSTEM | 用于产生对执行环境的请求 |
      | EBREAK  | 0    | PRIV   | 0    | SYSTEM | 调试器用它返回调试环境  |
    
      ECALL指令只是触发一个当前特权级下的exception，并不执行其它操作。
    
      注：ECALL可以在各自的特权级下产生不同的exception,所以可以选择性地委派call exception。对于Unix-like操作系统来说，一个典型的用法是通过U-mode下的调用切换到S-mode。

#### 3. RV32E：RV32I的精简版，为嵌入式系统而设计

      1. 程序员模型：通用寄存器从31个(x1~x31)减少到15个(x1~x15)
      2. 指令集：与RV32I相同，但计时器和计数器伪指令不是必须的。
      3. 扩展：可进行M,A,C扩展，只有两种特权级(用户模式和机器模式)

#### 4. RV64I：RV32I的变体，只描述与RV32I的不同之处。

   1. 寄存器状态：寄存器扩展为64位，且支持64位用户地址空间。

   2. 整数计算指令(增加了9条特有指令)

      RV64I的指令长度也是32位的，操作的是64位数值。但也提供了变种指令来操作32位数值，这些指令的操作码后面都增加了后缀“W"，是RV64I特有的指令。

      | imm       | rs1  | funct3 | rd   | opcode    |
      | --------- | ---- | ------ | ---- | --------- |
      | imm[11:0] | src  | ADDIW  | dest | OP-IMM-32 |
      | imm[4:0]  | src  | SLLIW  | dest | OP-IMM-32 |
      | imm[4:0]  | src  | SRLIW  | dest | OP-IMM-32 |
      | imm[4:0]  | src  | SRAIW  | dest | OP-IMM-32 |

      | funct7  | rs2  | rs1  | funct3    | rd   | opcode |
      | ------- | ---- | ---- | --------- | ---- | ------ |
      | 0000000 | src2 | src1 | ADDW      | dest | OP-32  |
      | 0000000 | src2 | src1 | SLLW/SRLW | dest | OP-32  |
      | 0100000 | src2 | src1 | SUBW/SRAW | dest | OP-32  |

      ​

   3. Load和Store指令(增加了3条指令)

      增加LD(64位)，LWU(32位无符号)，SD(64位）三条指令。

   4. 系统指令：与RV32I完全相同，只是操作的对象换成了64位的。需要注意的是，伪指令rdcycle,rdtime,rdinstret分别读取的是相应寄存器的全部64位(cycle,time,instret计数器)，所以rdcycleh,rdtimeh,rdinstreth在RV64I里就不需要了，是非法的。

#### 5. RV128I

#### 6. M：整数乘除扩展

1. 乘法操作

   | funct7+rs2+rs1+funct3+rd+opcode | funct7 | funct3          | opcode | 意义                                  |
   | ------------------------------- | ------ | --------------- | ------ | ----------------------------------- |
   | MUL/MULH\[[S]U] rd, rs1, rs2    | MULDIV | MUL/MULH\[[S]U] | OP     | MUL将乘法的低位放入目标寄存器，MULH则是放高位          |
   | MULW rd, rs1, rs2               | MULDIV | MULW            | OP-32  | 仅用于RV64，计算源寄存器的低32位，再用MUL指令获取高32位的值 |

   ​

2. 除法操作

   | funct7+rs2+rs1+funct3+rd+opcode | funct7 | funct3          | opcode | 意义                     |
   | ------------------------------- | ------ | --------------- | ------ | ---------------------- |
   | DIV[U]/REM[U] rd, rs1, rs2      | MULDIV | DIV[U]/REM[U]   | OP     | rs1/rs2。DIV提供商，REM提供余数 |
   | DIV[U]W/REM[U]W rd, rs1, rs2    | MULDIV | DIV[U]W/REM[U]W | OP-32  | 仅用于RV64,源寄存器的低32位相除。   |

   ​

#### 7. A：原子扩展

原子指令是原子性地“读-改-写”内存，以支持同一内存空间中多个硬件线程之间的同步。共有两种形式的原子指令，load-reserved/store-conditional指令和原子“获取-操作”内存指令。它们都支持各种内存一致性排序，都支持RCsc内存一致性模型。

1. 原子指令的顺序

   基本RISCV ISA使用的是宽松的内存模型，用FENCE指令来施加额外的次序约束。

   为了更有效地支持一致性，每个原子指令都有两个位，aq和rl，用于指定其他的RISC-V harts所观察到的额外的内存排序约束。

   如果两个比特都清空了，那么就不会对原子内存操作施加额外的排序约束。如果只设置了aq位，则将原子内存操作视为**获取访问**，即：在**获取访问**操作之前，在这个RISC-V hart中，没有任何后续的内存操作可以被观察到。如果只设置rl位，则将原子内存操作视为一个**释放访问**，即：在这个RISC-V hart中，任何早期内存操作之前，释放内存操作都不能观察到。如果aq和rl位都设置了，则在所有早期的内存操作之前或后期的内存操作之后，在相同的RISCV hart上，原子内存操作是顺序一致的且不能被观察到，而只能被全局次序中的其它hart观察到。

2. load-reserved/store-conditional指令

   | funct5+aq+rl+rs2+rs1+funct3+rd+opcode | funct5 | rs2  | opcode | 意义                                                         |
   | ------------------------------------- | ------ | ---- | ------ | ------------------------------------------------------------ |
   | lr rd, (rs1)                          | LR     | 0    | AMO    | 原子交换的前半部分：load且将内存地址注册为reservation        |
   | sc rd, rs2, (rs1)                     | SC     | src  | AMO    | 原子交换的后半部分：(rs1)=rs2如果内存地址的reservation有效,成功则rd为0,失败则rd非0. |

   在完成上一个LR之前,一个SC指令不能被另一个RISC-V hart观察到。由于LR/SC序列的原子性，在LR和一个成功的SC之间，任何hart的内存操作都不能被观测到。LR/SC序列可以通过在SC指令上设置aq位来赋予**获取语义**。LR/SC序列可以通过在LR指令上设置rl位来赋予**释放语义**。在LR指令上设置aq和rl位，并在SC指令上设置aq位，使得LR/SC序列与其他顺序一致的原子操作顺序一致。

   如果LR/SC的位均没有设置，则LR/SC序列是可以观测到的。这适用于并行下降操作。

3. 原子内存操作atomic memory operation(AMO)

   | funct5+aq+rl+rs2+rs1+funct3+rd+opcode | funct5        | opcode | 意义                         |
   | ------------------------------------- | ------------- | ------ | -------------------------- |
   | amoswap.w/d rd, rs2, (rs1)            | AMOSWAP.W/D   | AMO    | rd=(rs1);                  |
   | amoadd.w/d rd, rs2, (rs1)             | AMOADD.W/D    | AMO    | rd=(rs1);(rs1)=rd+rs2      |
   | amoand.w/d rd, rs2, (rs1)             | AMOAND.W/D    | AMO    | rd=(rs1);(rs1)=rd&rs2      |
   | amoor.w/d rd, rs2, (rs1)              | AMOOR.W/D     | AMO    | rd=(rs1);(rs1)=rd\|rs2     |
   | amoxor.w/d rd, rs2, (rs1)             | AMOXOR.W/D    | AMO    | rd=(rs1);(rs1)=rd^rs2      |
   | amomax[u].w/d rd, rs2, (rs1)          | AMOMAX[U].W/D | AMO    | rd=(rs1);(rs1)=max(rd,rs2) |
   | amomin[u].w/d rd, rs2, (rs1)          | AMOMIN[U].W/D | AMO    | rd=(rs1);(rs1)=min(rd,rs2) |

   原子内存操作(AMO)指令为多处理器同步执行"读-修改-写"操作，并使用r类型指令格式进行编码。这些AMO指令从rs1代表的内存地址原子地加载数据值，将该值放入rd，对rd的值和rs2的值进行操作，然后将结果存储回rs1的内存地址。AMOs可以在内存中操作64位(仅限RV64)或32位的数据。对于RV64，AMOs总是对rd中的值进行符号扩展。rs1中的地址必须与操作数的大小自然对齐(即，8字节对齐64位字，4字节对齐32位字)，否则就会造成未对齐地址异常。

   支持的操作有swap, add, and, or, xor, 符号和非符号整数的maximum和minimum。除了排序约束，这些AMOs也可以实现并行下降操作，一般会通过写入x0的方式来丢弃返回值。

   为了帮助实现多处理器同步，AMOs可选地提供发布一致性语义。如果设置了aq位，那么在这个riscv - v hart中，就不能观察到在AMO之前发生的内存操作。相反，如果设置了rl位，那么其他的RISC-V harts将不会在这个riscv - v hart的内存访问之前观察AMO。

#### 8. F：浮点扩展

1. F寄存器

   F扩展增加了32个浮点寄存器和1个浮点控制和状态寄存器fcsr。浮点寄存器是f0-f31，每个32位宽。浮点控制和状态寄存器则包含了浮点单元的操作模式和异常状态。

2. 浮点控制和状态寄存器

3. NaN的生成与传播

4. 低能的算术

5. 单精度Load和Store指令

6. 单精度浮点计算指令

7. 单精度浮点转换和移动指令

8. 单精度浮点比较指令

9. 单精度浮点类型指令

#### 9. D：双精度扩展

1. D寄存器

   D将32个浮点寄存器扩展到64位，F寄存器现在可以保存32位或64位浮点值了。

2. NaN Boxing of Narrower Values

3. 双精度Load和Store指令

4. 双精度浮点计算指令

5. 双精度浮点转换和移动指令

6. 双精度浮点比较指令

7. 双精度浮点类型指令

#### 10. Q：四精度扩展

#### 11. L：十进制浮点扩展

#### 12. C：压缩指令扩展

#### 13. B：位操作扩展

#### 14. J：动态翻译语言扩展

#### 15. T：事物内存扩展

#### 16. P：封装的单指令多数据流扩展

#### 17. V：向量操作扩展

#### 18. N：用户级中断扩展

#### 19. G：RV32/64G 指令集列表

#### 20. RISC-V汇编程序员手册

- 寄存器在标准调用规约中的角色

  | 寄存器    | ABI名字 | 描述            | 由谁保存 |
  | ------ | ----- | ------------- | ---- |
  | x0     | zero  | 固定为0          | 无    |
  | x1     | ra    | 返回地址          | 调用者  |
  | x2     | sp    | 栈指针           | 被调用者 |
  | x3     | gp    | 全局指针          | 无    |
  | x4     | tp    | 线程指针          | 无    |
  | x5     | t0    | 临时寄存器/备用连接寄存器 | 调用者  |
  | x6-7   | t1-2  | 临时寄存器         | 调用者  |
  | x8     | s0/fp | 保存寄存器/桢指针     | 被调用者 |
  | x9     | s1    | 保存寄存器         | 被调用者 |
  | x10-11 | a0-1  | 函数参数/返回值      | 调用者  |
  | x12-17 | a2-7  | 函数参数          | 调用者  |
  | x18-27 | s2-11 | 保存寄存器         | 被调用者 |
  | x28-31 | t3-6  | 临时寄存器         | 调用者  |

- 普通伪指令

  | 伪指令                          | 对应的基本指令                                  | 意义                         |
  | ---------------------------- | ---------------------------------------- | -------------------------- |
  | la rd, symbol                | auipc rd, symbol[31:12] ; addi rd, rd, symbol[11:0] | rd = pc + symbol，载入地址      |
  | l{b\|h\|w\|d} rd, symbol     | auipc rd, symbol[31:12]; l{b\|h\|w\|d} rd, symbol[11:0]\(rd) | 载入全局地址                     |
  | s{b\|h\|w\|d} rd, symbol, rt | auipc rt, symbol[31:12]; s{b\|h\|w\|d} rd, symbol[11:0]\(rt) | 存储全局地址                     |
  | nop                          | addi x0, x0, 0                           | 空操作                        |
  | li rd, immediate             | 无穷序列                                     | rd = immediate，存储立即数       |
  | mv rd, rs                    | addi rd, rs, 0                           | rd = rs + 0,复制寄存器          |
  | not rd, rs                   | xori rd, rs, -1                          | rd = rs ^ -1, rs的补码        |
  | neg rd, rs                   | sub rd, x0, rs                           | rd = 0 - rs, 补码            |
  | negw rd, rs                  | subw rd, x0, rs                          | rd = 0 - rs, 补码            |
  | sext.w rd, rs                | addiw rd, rs, 0                          | rd = rs + 0, 符号扩展          |
  | seqz rd, rs                  | sltiu rd, rs, 1                          | rd = (rs == 0) ? 1 : 0;    |
  | snez rd, rs                  | sltu rd, x0, rs                          | rd = (rs != 0) ? 1 : 0;    |
  | sltz rd, rs                  | slt rd, rs, x0                           | rd = (rs < 0) ? 1 : 0;     |
  | sgtz rd, rs                  | slt rd, x0, rs                           | rd = (rs > 0) ? 1 : 0;     |
  | beqz rs, offset              | beq rs, x0, offset                       | if (rs == 0), branch       |
  | bnez rs, offset              | bne rs, 0x, offset                       | if( rs != 0), branch       |
  | blez rs, offset              | bge x0, rs, offset                       | if(rs <= 0), branch        |
  | bgez rs, offset              | bge rs, x0, offset                       | if(rs >= 0), branch        |
  | bltz rs, offset              | blt rs, x0, offset                       | if(rs < 0), branch         |
  | bgtz rs, offset              | blt x0, rs, offset                       | if(rs > 0), branch         |
  | bgt rs, rt, offset           | blt rt, rs, offset                       | if(rs > rt), branch        |
  | ble rs, rt, offset           | bge rt,rs, offset                        | if(rs <= rt), branch       |
  | bgtu rs, rt, offset          | bltu rt, rs, offset                      | if( rs > rt), branch       |
  | bleu rs, rt, offset          | bgeu rt, rs, offset                      | if(rs <= rt), branch       |
  | j offset                     | jal x0, offset                           | pc += offset               |
  | jal offset                   | jal x1, offset                           | pc += offset; x1 = pc + 4; |
  | jr rs                        | jalr x0, rs, 0                           | pc = rs + 0                |
  | jalr rs                      | jalr x1, rs, 0                           | pc = rs + 0; x1 = pc + 4;  |
  | ret                          | jalr x0, x1, 0                           | pc = x1 + 0                |
  | call offset                  | auipc x6, offset[31:12]; jalr x1, x6, offset[11:0] | pc += offset; x1 = pc + 4; |
  | tail offset                  | auipc x6, offset[31:12]; jalr x0, x6, offset[11:0] | pc += offset               |
  | fence                        | fence iorw, iorw                         | 隔离内存和IO                    |

  ​

- 访问CSR的伪指令

  | 伪指令             | 对应的基本指令                  | 意义                        |
  | --------------- | ------------------------ | ------------------------- |
  | rdinstret[h] rd | csrrs rd, instret[h], x0 | rd = instret[h]，读取指令退休计数器 |
  | rdcycle[h] rd   | csrrs rd, cycle[h], x0   | rd = cycle[h]，读取循环计数器     |
  | rdtime[h] rd    | csrrs rd, time[h], x0    | rd = time[h], 读取实时时钟      |
  | csrr rd, csr    | csrrs rd, csr, x0        | rd = csr, 读取csr           |
  | csrw csr, rs    | csrrw x0, csr, rs        | csr = rs, 写入csr           |
  | csrs csr, rs    | csrrs x0, csr, rs        | csr \|= rs, csr置位         |
  | csrc csr, rs    | csrrc x0, csr, rs        | csr &= !rs, csr清位         |
  | csrwi csr, imm  | csrrwi x0, csr, imm      | csr = imm, 立即数写入csr       |
  | csrsi csr, imm  | csrrsi x0, csr, imm      | csr \|= imm, 立即数置位csr     |
  | csrci csr, imm  | csrrci x0, csr, imm      | csr &= !imm, 立即数清位csr     |

  ​

#### 21. 扩展RISC-V

#### 22. ISA子集命名约定

#### 23. 历史与致谢

### RISC-V手册下卷(riscv-privileged-v1.10)

#### 1. 介绍

   1. 硬件平台术语

      hart：硬件线程hardware thread。

      核心(core)：包含独立取指单元的组件。

      协处理器(coprocessor)：是与RISC-V核心相连的一个单元，且主要被RISC-V指令流序列化，但包含了一些额外的体系结构状态和指令集扩展，相对于主RISC-V指令流来说可能会有些有限的自治。

      加速器(accelerator)：一个非可编程的固定功能单元，或可以自治工作、但专门用于某项任务的核心。

   2. 特权软件栈术语

      AEE：应用程序执行环境

      ABI：应用程序二进制接口

      SBI：管理员二进制接口

      SEE：管理员执行环境

      HBI：管理程序二进制接口

      HEE：管理程序执行环境

      HAL：硬件抽象层

   3. 特权级

      | 级别   | 编码   | 名字       | 缩写   | 特点          |
      | ---- | ---- | -------- | ---- | ----------- |
      | 0    | 00   | 用户/应用    | U    | 应用程序        |
      | 1    | 01   | 管理员      | S    | 操作系统        |
      | 2    | 10   | Reserved |      |             |
      | 3    | 11   | 机器       | M    | 必需的，可信的，最高级 |

   4. 调试模式：用以支持片外调试或制造测试，可以认为是一种比机器级更高的特权级。

#### 2. 控制和状态寄存器(CSR)
   1. CSR地址映射约定

   2. CSR列表

      **用户级**:Trap Setup, Trap Handling, Floating-Point, Counter/Timers

      **管理员级**:Trap Setup, Trap Handling, Protection & Translation

      **机器级**:Information, Trap Setup, Trap Handling, Protection & Translation, Counter/Timers, Counter Setup

      **调试级**:Debug/Trace(与机器级共享), Debug Mode

   3. CSR域的说明

      **WIRI**：写忽略读忽略，用于保留的只读域。

      **WPRI**：写保留读忽略，用于保留的读写域。

      **WLRL**：写合法读合法，用于部分位有用的保留域。

      **WARL**：写任意读合法，用于部分位有用的保留域。

#### 3. 机器级ISA

##### 3.1机器级CSR

- **misa**：是一个WARL寄存器，它会报告hart支持的ISA。

  | XLEN-1 ~ XLEN-2 | XLEN-3 ~ 26    | 25 ~ 0                  |
  | --------------- | -------------- | ----------------------- |
  | MXL[1:0] (WARL) | WIRI           | Extensions[25:0] (WARL) |
  | 2 bits          | XLEN-28 (bits) | 26 bits                 |

  **MXL**字段编码本机I指令的宽度，1代表32位，2代表64位，3代表128位。**Extensions**字段用单个字母占据1位的方式表明是否存在相应的标准扩展（比如A在0位，B在1位，Z在25位）。**I**表明RV32I或RV64I或RV128I，**E**表明RV32E，如果**I**和**E**同时存在应选择**I**。由于**G**和**IMAFD**重复，为避免冗余G表示除IMAFD外的额外扩展。**U**表示支持用户模式，**S**表示支持Supervisor模式。**X**表示存在非标准扩展。

- mvendorid：机器供应商ID寄存器。

- martid：机器架构ID寄存器。提供了hard的基本微架构的编码。

- mimpid：机器实现ID寄存器。提供了处理器实现版本的唯一编码。

- mhartid：包含了运行代码的硬件线程的id值，这个寄存器是只读的。在多处理器系统中，HardID的编号不一定的连续的，但一定有一个是0。

- **mstatus**：追踪和控制硬件线程的当前操作状态。

  | 名称     | RV32    | RV64,RV128  | 意义                   |
  | -------- | ------- | ----------- | ---------------------- |
  | UIE      | 0       | 0           | 1开启用户中断          |
  | SIE      | 1       | 1           | 1开启管理员中断        |
  | WPRI     | 2       | 2           | 保留                   |
  | MIE      | 3       | 3           | 1开启机器中断          |
  | UPIE     | 4       | 4           | 进入自陷前UIE的值      |
  | SPIE     | 5       | 5           | 进入自陷前SIE的值      |
  | WPRI     | 6       | 6           | 保留                   |
  | MPIE     | 7       | 7           | 进入自陷前MIE的值      |
  | SPP      | 8       | 8           | 进入S-mode之前的特权级 |
  | WPRI     | 9 ~ 10  | 9 ~ 10      | 保留                   |
  | MPP[1:0] | 11 ~ 12 | 11 ~ 12     | 进入M-mode之前的特权级 |
  | FS[1:0]  | 13 ~ 14 | 13 ~ 14     |                        |
  | XS[1:0]  | 15 ~ 16 | 15 ~ 16     |                        |
  | MPRV     | 17      | 17          |                        |
  | SUM      | 18      | 18          |                        |
  | MXR      | 19      | 19          |                        |
  | TVM      | 20      | 20          |                        |
  | TW       | 21      | 21          |                        |
  | TSR      | 22      | 22          |                        |
  | WPRI     | 23 ~ 30 | 23 ~ 31     |                        |
  | UXL[1:0] |         | 32 ~ 33     | 同misa寄存器的MXL域    |
  | SXL[1:0] |         | 34 ~ 35     | 同misa寄存器的MXL域    |
  | WPRI     |         | 36 ~ XLEN-2 |                        |
  | SD       | 31      | XLEN -1     |                        |

  **TSR,TW,TVW用来支持虚拟化** 。**TSR** (Trap SRET)支持拦截Supervisor异常返回指令，SRET。**TW** (Timeout Wait) 支持拦截WFI指令。**TVM**(Trap Virtual Memory) 支持拦截supervisor虚拟内存管理操作。当TVM=1时，在S模式下尝试读取或写入satp CSR或执行SFENCE.VMA指令会引发非法指令异常。当TVM=0时，这些操作在s模式下是允许的。**MPRV,MXR,SUM用来支持内存权限** 。**MXR** (Make eXecutable Readable)修改了加载访问虚拟内存的特权，当其为0时只有标记为可读的页面才可以加载，当其为1时从标记为可读或可执行的页面均可加载 。**SUM**(permit Supervisor User Memory access)修改S模式加载、存储和取指访问虚拟内存的特权。当其为0时，S模式不可以访问U模式的页面，当其为1时这样的访问是允许的。**MPRV** (Modify PRiVilege)修改在所有特权模式下加载和存储执行的权限级别，当其置位时加载和存储内存地址就好像当前特权模式为MPP一样，指令地址的转换和保护不受影响。如果不支持用户模式，则MPRV会被硬连接为1。**FS、XS和SD用来扩展上下文状态** 。**FS**字段编码浮点单元的状态，包括CSR fcsr和浮点数据寄存器f0-f31，**XS**字段编码额外的用户模式扩展和关联状态的状态。**SD**位是一个只读的位，它总结了FS字段或XS字段是否表示存在某个脏状态，那种状态需要将扩展的用户上下文保存到内存中。如果XS和FS都被固定为零，那么SD也总是为零。

- mtvec：机器自陷向量基址寄存器，记录自陷向量的配置，由向量基址和向量模式组成。

  | 位         | 名称           | 作用                                                         |
  | ---------- | -------------- | ------------------------------------------------------------ |
  | 2 ~ XLEN-1 | BASE[XLEN-1:2] | 中断例程的高XLEN-2位，要4字节对齐                            |
  | 0 ~ 1      | MODE           | 0中断入口仅由BASE决定，1异步中断的地址为BASE+4*cause，2和3为保留位 |

- medeleg和mideleg：机器自陷授权寄存器(机器异常授权寄存器和机器中断授权寄存器)

  默认情况下，所有特权级的内陷都是在机器级处理的。为提高性能，使用medeleg和mideleg寄存器来表明某些异常和中断可以由较低的权限级别直接处理。在三种特权级都具备的系统中，在medeleg或mideleg中设置相应的位，会将内陷(S模式或U模式)授权给S模式内陷处理程序。

- mip,mie：mip的意思是有未处理的中断，mie的意思是中断使能。

  sip/sie, uip/uie寄存器是mip/mie寄存器的受限视图。

  | 位           | mip  | mie  | 意义                                                         |
  | ------------ | ---- | ---- | ------------------------------------------------------------ |
  | 12 ～ XLEN-1 | WIRI | WPRI | 保留区                                                       |
  | 11           | MEIP | MEIE | 外中断挂起或使能(机器级)                                     |
  | 10           | WIRI | WPRI | 保留区                                                       |
  | 9            | SEIP | SEIE | 外中断挂起或使能(管理员级)                                   |
  | 8            | UEIP | UEIE | 外中断挂起或使能(用户级)                                     |
  | 7            | MTIP | MTIE | 计时器中断挂起或使能(机器级，只读，只能通过写mtimecmp寄存器来清零) |
  | 6            | WIRI | WPRI | 保留区                                                       |
  | 5            | STIP | STIE | 计时器中断挂起或使能(管理员级)                               |
  | 4            | UTIP | UTIE | 计时器中断挂起或使能(用户级)                                 |
  | 3            | MSIP | MSIE | 软件中断挂起或使能(机器级)                                   |
  | 2            | WIRI | WPRI | 保留区                                                       |
  | 1            | SSIP | SSIE | 软件中断挂起或使能(管理员级)                                 |
  | 0            | USIP | USIE | 软件中断挂起或使能(用户级)                                   |

  指令手册没有明确解释，但我**推测**外中断是外部设备引发的中断，软件中断是处理器和内存引发的中断，而计时器中断应属于外中断，它被单独分出来应该是基于计时器在进程调度中的重要作用。

  如果一个中断是全局使能的，且mip和mie相应位被设置，则那个中断将会被获取。默认情况下，如果hart的当前特权级小于M，或者当前特权级是M且mstatus寄存器的MIE位被设置，则M模式下的中断是全局使能的。当mideleg相应位被设置，如果hart的当前特权级等于授权的特权级(U或S)且当前特权级的中断使能位被设置(mstatus寄存器的SIE或UIE位)，或者当前特权级低于授权特权级，则中断被认为是全局使能的。

- mtime, mtimecmp

  mtime是一个实时计数器，表现为内存映射的寄存器。不论是RV32, RV64还是RV128，mtime都是64位的。

  mtimecmp是一个64位内存映射的计时器比较寄存器。当mtime中的值大于或等于mtimecmp时，将触发计时器中断。

- 硬件性能管理器

- **[m|h|s]counteren**：寄数器使能。将硬件性能监控计数器的可用性控制到下一个最低特权模式。

  | 31    | ...  | 3     | 2     | 1     | 0     |
  | ----- | ---- | ----- | ----- | ----- | ----- |
  | HPM31 | ...  | HPM3  | IR    | TM    | CY    |
  | 1 bit | ...  | 1 bit | 1 bit | 1 bit | 1 bit |

  **CY**代表cycle寄存器，**TM**代表time寄存器，**IR**代表instret寄存器(即指令消耗寄存器)，**HPM**代表hpmcounter寄存器(即性能监控计数器)。当相应位清零后，在更低特权级下访问相应的寄存器会非法指令异常。

- mscratch：记录机器模式硬件线程上下文的地址，当进入机器模式下的中断例程时可以与用户寄存器切换

- mepc：机器异常程序计数器

- mcause：机器原因计数器

  | 位          | 名称           | 作用                             |
  | ----------- | -------------- | -------------------------------- |
  | XLEN-1      | Interrupt      | 由中断引发则置1，由异常引发则置0 |
  | 0 ～ XLEN-2 | Exception Code | 引发中断的编码，详见下表         |

  | 编码  | Interrupt位为1   | Interrupt位为0      |
  | ----- | ---------------- | ------------------- |
  | 0     | 用户软件中断     | 指令地址未对齐      |
  | 1     | 管理员软件中断   | 指令访问错误        |
  | 2     | 保留             | 非法指令            |
  | 3     | 机器软件中断     | 断点                |
  | 4     | 用户计时器中断   | Load地址未对齐      |
  | 5     | 管理员计时器中断 | Load访问错误        |
  | 6     | 保留             | Store/AMO地址未对齐 |
  | 7     | 机器计时器中断   | Store/AMO访问错误   |
  | 8     | 用户外中断       | U-mode下的环境调用  |
  | 9     | 管理员外中断     | S-mode下的环境调用  |
  | 10    | 保留             | 保留                |
  | 11    | 机器外中断       | M-mode下的环境调用  |
  | 12    | \>=12 保留       | 指令页错误          |
  | 13    |                  | Load页错误          |
  | 14    |                  | 保留                |
  | 15    |                  | Store/AMO页错误     |
  | \>=16 |                  | 保留                |

- mtval：机器陷阱值寄存器，当自陷发生在M-mode时，mtval通过写入异常信息来辅助软件处理这个自陷。它用来取代之前版本的mbadaddr寄存器。

##### 3.2 机器模式特权指令

| funct12 | rs1  | funct3 | rd   | opcode | 意义                                   |
| ------- | ---- | ------ | ---- | ------ | -------------------------------------- |
| ECALL   | 0    | PRIV   | 0    | SYSTEM | 向更高特权级发起请求                   |
| EBREAK  | 0    | PRIV   | 0    | SYSTEM | 将控制转回调试环境                     |
| xRET    | 0    | PRIV   | 0    | SYSTEM | 在处理完自陷后返回，将xepc的值放到pc中 |
| WFI     | 0    | PRIV   | 0    | SYSTEM | 等待中断                               |

##### 3.3 Reset

一经重置，hart的特权模式就置为M。

##### 3.4 不可屏蔽中断Non-Maskable Interrupts(NMI)

不可屏蔽中断仅用于硬件错误的情况。

##### 3.5 物理内存特性 (**PMA**)

物理内存映射包含多种地址范围，一些对应到内存区域，一些对应到内存映射控制寄存器，一些则对应到地直空间的空洞。一些内存区域可能不支持读、写或执行，一些可能不支持子字或子块访问，一些可能不支持原子操作，一些可能不支持缓存一致性或可能有不同的内存模型。相似地，内存映射控制寄存器在访问宽度、对原子操作的支持和读写访问是否有副作用上也各不相同。在RISCV系统里，物理地址空间的这些属性和功能被称为物理内特性(PMA)。

PMAs是基础硬件的固有属性，在系统运行时基本不会改变。一些内存区域的PMAs在芯片设计时就已经确定，例如片上ROM。另一些是在电路板设计的时候确定的。芯片外总线也可能支持冷插拔或热插拔的设备。一些设备可能支持在运行的时候配置，这意味着不同的PMAs。

##### 3.6 物理内存保护 (**PMP**)

为了支持安全运行和抑制错误，最好限制运行在某个核上的程序对物理地址的访问。物理内存保护(PMP)单元提供了单个hart上机器模式下的控制寄存器，以设置在指定物理内存区域上的访问权限(read, write, execute)。PMP检查和PMA检查是并行的。

PMP访问控制的粒度是因平台而异的，即使在一个平台内也可能因物理内存区域而不同，但标准PMP编码支持小如4字节的区域。一些区域的特权级可以写死到硬件上，比如一些区域可以只在机器模式下可见。

当hart运行在S或U模式，mstatus寄存器的MPRV已置位且MPP域包含S或U，对于loads或stores来说PMP检查会应用于所有访问。PMP检查也适用于虚拟地址转换的页表访问，有效的特权模式为S。另外，PMP检查还可应用于M模式的访问，这种情况下PMP寄存器是锁定的，以便M模式下的软件也不可以修改它们。实际上，PMP可以将权限授予S或U模式，还可以从M模式撤销权限。

PMP违规总是精确地限制在处理器上。

- PMP CSRs

  PMP入口由一个8位配置寄存器和一个XLEN位地址寄存器描述。一共支持16个PMP入口。任意PHP入口被实现之后，所有PMP CSRs都必须被实现。PMP CSRs只能在M模式下访问。

  对于RV32,16个PMP入口的设置(pmp0cfg-pmp15cfg)保存在4个寄存器中pmpcfg0-pmpcfg3。对于RV64,16个入口保存在2个寄存器中pmpcfg0,pmpcfg2。

  PMP地址寄存器是pmpaddr0-pmpaddr15。对于RV32，PMP地址寄存器编码34位物理地址的33-2位。对于RV64,PMP地址寄存器编码56位物理地址的55-2位。

  配置寄存器的R，W，X位设置后，分别代表PMP入口允许读，写和指令执行。如下所示：

  | L(WARL) | (WIRL) | A(WARL) | X(WARL) | W(WARL) | R(WARL) |
  | ------- | ------ | ------- | ------- | ------- | ------- |
  | 1 bit   | 2 bits | 2 bits  | 1 bit   | 1 bit   | 1 bit   |

- 地址匹配

  PMP配置寄存器的A字段编码了地址匹配的模式。如下表所示：

  | A    | 名称  | 描述                                             |
  | ---- | ----- | ------------------------------------------------ |
  | 0    | OFF   | 空区域(无效，不匹配任何地址)                     |
  | 1    | TOR   | Top Of Range                                     |
  | 2    | NA4   | Naturally Aligned 4-byte region                  |
  | 3    | NAPOT | Naturally Aligned power-of-two region(>=8 bytes) |

  NAPOT的对齐范围由对应地址寄存器的低位来决定。如下表所示：

  | pmpaddr     | pmpcfg.A | 匹配大小              |
  | ----------- | -------- | --------------------- |
  | aaaa...aaaa | NA4      | 4 byte NAPOT          |
  | aaaa...aaa0 | NAPOT    | 8 byte NAPOT          |
  | aaaa...aa01 | NAPOT    | 16 byte NAPOT         |
  | ...         | ...      | ...                   |
  | a011...1111 | NAPOT    | 2^(XLEN+1) byte NAPOT |
  | 0111...1111 | NAPOT    | 2^(XLEN+2) byte NAPOT |

  如果选择TOR，相关联的地址寄存器即为地址范围的顶部，前面的地址寄存器即为地址范围的底部。如果PMP入口i的A字段设置为TOR，则入口匹配任意地址a(pmpaddr ~i-1~ <= a < pmpaddr~i~)。当i为0时，a的范围为(a < pmpaddr~0~)。

- 锁定和特权模式

  L位表示PMP入口被锁定。即，写配置寄存器和相关联的地址寄存器的操作将无效。只能通过重置系统的方法来解除锁定。另外，如果pmp*i*cfg.A被置为TOR，写pmpaddr*i-1*的操作将无效。

  除了锁定PMP入口，L位还表示R/W/X权限是否是强制的。当L位为1时，这些权限将强制用于所有特权模式。当L位为0时，匹配PMP入口的所有M模式访问都被允许，R/W/X权限仅适用于S和U模式。

- 优先权与匹配逻辑

  PMP入口的优先级是静态的。最低编号的PMP入口匹配任意字节的访问，它决定了访问是否成功。匹配的PMP入口必须匹配任意字节的访问，否则访问失败，这与L, R, W和X位无关。例如，如果PMP入口被设置为匹配4字节范围0xC－0xF，则对范围0x8-0xF的8字节访问将失败，假如那个PMP入口是匹配这些地址的最高优先级入口。

  如果PMP入口匹配访问的所有字节，L、R、W和X位决定访问是否成功。

  如果PMP入口匹配一个M模式访问，则此访问成功。

  错误的访问将生成一个load，store或指令访问异常。

  在某些实现里，未对齐的load, store和取指可能被分解为多个访问，在访问异常发生前其中的某些访问可能成功。

#### 4. 管理员级ISA

   1. 管理员CSR：

      - sstatus: 管理员状态寄存器，追踪处理器当前的操作状态，是XLEN位R/W寄存器。是mstatus寄存器的子集。

        | 位          | 名称 | 意义                                   |
        | ----------- | ---- | -------------------------------------- |
        | XLEN-1      | SD   |                                        |
        | 20 ~ XLEN-2 | WPRI | 保留                                   |
        | 19          | MXR  |                                        |
        | 18          | SUM  |                                        |
        | 17          | WPRI | 保留                                   |
        | 15 ~ 16     | XS   |                                        |
        | 13 ~ 14     | FS   |                                        |
        | 9 ~ 12      | WPRI | 保留                                   |
        | 8           | SPP  | 进入S-mode前hart的特权级。0:U, 1:S     |
        | 6 ~ 7       | WPRI | 保留                                   |
        | 5           | SPIE | 进入S-mode前SIE的值。中断返回时写回SIE |
        | 4           | UPIE | 进入U-mode前UIE的值。中断返回时写回UIE |
        | 2 ~ 3       | WPRI | 保留                                   |
        | 1           | SIE  | S-mode下的中断。1：使能，0：停止       |
        | 0           | UIE  | U-mode下的中断。1：使能，0：停止       |

        - UIE位开启或停止用户模式的中断。
        - UPIE位的功能和SPIE位类似。

      - stvec：存放中断服务例程(ISR)的入口地址(BASE)和模式(MODE)

        | XLEN-1 ~ 2            | 1 ~ 0       |
        | --------------------- | ----------- |
        | BASE[XLEN-1:2]\(WLRL) | MODE\(WARL) |

        注意：BASE不包含最低两位，因为要求此地址4字节对齐，即最低两位一定是0。

      - sip, sie：管理员中断寄存器，sip挂起中断，sie使能中断。它们是mip和mie的子集。

        | 位          | sip                              | sie                        |
        | ----------- | -------------------------------- | -------------------------- |
        | 10 ~ XLeN-1 | WIRI(保留)                       | WPRI(保留)                 |
        | 9           | SEIP                             | SEIE                       |
        | 8           | UEIP：用户特权级的外部中断被挂起 | UEIE：使能用户级外部中断   |
        | 6 ~ 7       | WIRI(保留)                       | WPRI(保留)                 |
        | 5           | STIP                             | STIE                       |
        | 4           | UTIP                             | UTIE                       |
        | 2 ~ 3       | WIRI(保留)                       | WPRI(保留)                 |
        | 1           | SSIP：触发管理员级软件中断       | SSIE：使能管理员级软件中断 |
        | 0           | USIP                             | USIE                       |

        这些位的名称都是4个字母的。第1个字母表示特权级：S，管理员级;U，用户级。第2个字母表示中断类型：E(External)，外部中断；T(Timer)，定时器中断；S(Software)，软件中断。第4个字母表示挂起(P)还是使能(E)。

      - time, cycle, instret ：与用户级程序共用这些寄存器。

      - scounteren：详见机器级ISA对应的寄存器。控制硬件性能监控计数器在U-mode下的可用性。1则可用，0则不可用。可控制的寄存器有：CY(cycle)，TM(time), IR(instret), HPMn(hpmcountern).

      - sscratch：记录管理员模式中断上下文的指针

      - sepc：记录引起异常的指令的地址

      - scause：各种中断的来源，interrupt位为0表示异常，为1表示中断

      - stval：陷阱值寄存器。引发错误的内存地址会保存在这个寄存器中。

      - satp：地址转换与内存保护寄存器。

        | 字段   | RV32                  | RV64                          | 意义       |
        | ---- | --------------------- | ----------------------------- | -------- |
        | MODE | 1 bit (0:bare,1:Sv32) | 4 bits (0:bare,8:Sv39,9:Sv48) | 地址转换方案   |
        | ASID | 9 bits                | 16 bits                       | 地址空间标识符  |
        | PPN  | 22 bits               | 44 bits                       | 根页表的物理页号 |

        注意，修改satp寄存器并不意味着给页表更新和地址转换之间加上了次序约束。所以，在修改satp寄存器之前，应该先执行sfence.vma指令。

   2. 管理员指令

      除了SRET指令，还增加了一条新的管理员级的指令。

      | funct7     | rs2  | rs1   | funct3 | rd   | opcode | 意义         |
      | ---------- | ---- | ----- | ------ | ---- | ------ | ---------- |
      | SFENCE.VMA | asid | vaddr | PRIV   | 0    | SYSTEM | 更新内存管理数据结构 |

   3. Sv32:基于页的32位虚拟内存系统

      当Sv32被写入satp寄存器的MODE域时，就进入了32位的分页虚拟内存模式。Sv32基于RV32，其被设计为足以支持类Unix操作系统。

      - 寻址与内存保护

        Sv32将虚拟地址映射到物理地址的过程与X86是相同的，页表的基址放在satp寄存器内。所不同的是Sv32的虚拟地址是32位的，而物理地址是34位的。虚拟地址可以寻址4G的空间，而物理地址可寻址16G的空间。我猜测这样设计是为了支持虚拟化，即虚拟机上的不同系统可以使用不同的物理内存，互不干扰。

        | PPN[1]  | PPN[0]  | RSW    | D     | A     | G     | U     | X     | W     | R     | V     |
        | ------- | ------- | ------ | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
        | 12 bits | 10 bits | 2 bits | 1 bit | 1 bit | 1 bit | 1 bit | 1 bit | 1 bit | 1 bit | 1 bit |

        | 位       | 意义                                   |
        | ------- | ------------------------------------ |
        | V       | 1代表PTE有效，0代表PTE无效                    |
        | X, W, R | 可执行，可写，可读。全为0代表是指向下级页表的指针，否则就是叶子PTE  |
        | U       | 1代表U-mode可访问，0代表U-mode不可访问（仅用于叶子PTE） |
        | G       | 全局映射                                 |
        | A       | 虚拟页曾被访问过（仅用于叶子PTE）                   |
        | D       | 虚拟页曾被写入过（仅用于叶子PTE）                   |
        | RSW     | reserved                             |

      - 虚拟地址转换过程

        Sv32，Sv39，Sv48的转换算法都是一样的，不同的只有LEVELS和PTESIZE的值。LEVELS指的是vpn有几级，PTESIZE指的是PTE占几个字节。

   4. Sv39:基于页的39位虚拟内存系统

      Sv39支持39位的虚拟地址空间，其设计遵循Sv32的总体方案，本节仅介绍它们之间的不同点。Sv39基于RV64。

      - 寻址与内存保护

        Sv39的虚拟地址是64位的，但只有低39位有效，且高25位必须和第38位相等（注：这里说的第38位指的是从0开始计数）。从虚拟地址到物理地址共需3级页表的转换。

        每一级的PTE都有可能是叶子PTE，所以除了4K的页外，还有可能存在2M或1G的页，每种页都必须依照它自己的大小在虚拟层面和物理层面对齐。

   5. Sv48:基于页的48位虚拟内存系统

      Sv48支持48位的虚拟地址空间，用以弥补Sv39的不足。Sv48也基于RV64。支持Sv48的实现也应当支持Sv39。

      - 寻址与内存保护

        Sv48的虚拟地址占用了64位，但只有低48位是有效的，且高16位都必须和第47位相等（注，第47位的意思是从0开始计数，如果从1开始计数则是第48位）。从虚拟地址到物理地址共需4级页表的转换。

        每一级的PTE都有可能是叶子PTE，所以除了4K的页外，还可能有2M，1G，512G的页，每种页都必须依照它自己的大小在虚拟层面和物理层面对齐。

#### 5. Hypervisor扩展

#### 6. RISC-V特权指令列表

都是I-type类型的指令，它们是**环境调用和断点指令**`ecall`和`ebreak`，**陷阱返回指令**`uret`,`sret`,`mret`，**中断管理指令**`wfi`，**内存管理指令**`sfence.vma`。

#### 7. 平台级中断控制器(PLIC)

#### 8. 机器配置描述

#### 9. 历史
