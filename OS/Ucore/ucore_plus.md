#### 来源

[ucore_plus_riscv_g08](https://github.com/riscv-labs/OS2018spring-projects-g08)

#### qemu中运行

```
git clone https://github.com/riscv-labs/OS2018spring-projects-g08
cd OS2018spring-projects-g08/ucore
make menuconfig ARCH=riscv_64	# 将NUMA相关的3个选项都设为4,代表开启4核
make sfsimg
make sfsimg2
make
./uCore_test -d obj
```

##### 问题解决

- `undefined reference to 'mycpu'`

  master分支正在调试中，切换到smp分支即可。

#### FU540-C000中运行

##### 第一次尝试

1. 模仿Linux在FU540-C000运行，将kernel.img(bbl)输出为sd.img
2. 使用`dd if=sd.img of=/dev/XXX`命令将sd.img写到SD卡
3. 上电运行，使用`sudo minicom -D /dev/ttyUSB1`接收串口消息
4. 未能正常运行，只打印出logo信息，即`riscv-pk/bbl/riscv_logo.txt`里的内容。

#### 编译过程分析

- ucore编译结果放在`ucore/obj`目录
- bbl编译结果放在`riscv-pk/build`目录
- `ucore/obj/kernel.img`即为`riscv-pk/build/bbl`

