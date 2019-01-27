#### 简介

sodor实现了若干riscv核，它们都实现了RV32b的2.0版本，都不支持虚拟内存，只实现了m-mode的priv 1.10版本。它们使用的verilog文件是用chisel3生成的，它与C++一起生成和运行Sodor仿真器。

| 流水 |                                  |      |
| ---- | -------------------------------- | ---- |
| 1级  | ISA模拟器                        |      |
| 2级  | 演示流水                         |      |
| 3级  | 使用连续内存                     |      |
| 5级  | 可以在完全绕过和完全互锁之间切换 |      |
| 5级  | 基于micro-coded实现              |      |

#### 运行

##### 编译模拟器

```
git clone https://github.com/ucb-bar/riscv-sodor.git
cd riscv-sodor
git submodule update --init --recursive
```



#### 参考资料

[Learning journey](https://github.com/librecores/riscv-sodor/wiki/Learning-journey)