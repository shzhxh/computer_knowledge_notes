#### 二进制编码

SBI调用与标准的RISC-V函数调用完全相同，除了：

- 使用的控制转移指令是`ecall`而不是`call`
- `a7`编码了SBI扩展ID，这和标准UNIX系统调用ABI里的系统调用ID的编码是一致的。

许多SBI扩展还选择在`a6`编码一个额外的函数ID，这类似于许多UNIX系统中的`ioctl()`系统调用。这使得SBI扩展可以编码多个函数。

为了兼容，SBI扩展ID和SBI函数ID都编码为有符号32位整数。当在寄存器中传递时，它们遵循标准的RISC-V调用约定。

SBI函数必须返回`a0`和`a1`的值。`a0`返回的是错误码，这类似于返回一个结构体

```c
struct sbiret {
    long error;
    long value;
}
```

标准SBI错误码如下

| Error Type                | Value |
| ------------------------- | ----- |
| SBI_SUCCESS               | 0     |
| SBI_ERR_FAILED            | -1    |
| SBI_ERR_NOT_SUPPORTED     | -2    |
| SBI_ERR_INVALID_PARAM     | -3    |
| SBI_ERR_DENIED            | -4    |
| SBI_ERR_INVALID_ADDRESS   | -5    |
| SBI_ERR_ALREADY_AVAILABLE | -6    |

每个SBI函数都应当使用`unsigned long`作为数据类型。这样使得规范可以适用于所有的RISC-V ISA类型。如果数据被定义为32位，更高特权级的软件必须确保它只使用32位的数据。

如果SBI函数需要传递一组harts给更高的特权级，它必须使用下面定义的掩码。这适用于v0.2之后的任意扩展。

任何需要hart掩码的函数，都要传递如下两个参数：

- `unsigned long hart_mask`是一个包含hartids的位向量
- `unsigned long hart_mask_base`是起始hartid，它标记了位向量的起始位置

在单独的SBI函数调用里，可设置的最大hart数总是XLEN。如果低特权级需要传递的信息超过XLEN harts，它应该引用SBI函数调用的多个实例。`hart_mask_base`设置为-1表明`hart_mask`可以被忽略，此时所有可用的harts都应被考虑。

任何使用了hart掩码的函数，除了返回特定于函数的错误值，还可能返回如下错误值：

| Error Code            | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| SBI_ERR_INVALID_PARAM | `hart_mask_base`或`hart_mask`无效。如hartid没有被平台使能，或对管理员特权级不可用 |



#### SBI基本函数

基本SBI被设计地尽可能小。因此，它的功能仅是探测哪些SBI扩展可用，查询SBI的版本。基本函数必须被实现，所以没有定义错误返回。它们的扩展ID是0x10。

##### sbi_get_spec_version

```c
/* 函数ID:0
 * 返回当前SBI规范的版本。这个函数必须总是成功。
 * SBI规范的低24位是副号，接下来的7位是主号，第31位是0(保留位用于将来的扩展)。
 */
struct sbiret sbi_get_spec_version(void);
```

##### sbi_get_impl_id

```c
/* 函数ID:1
 * 返回当前SBI的实现ID。这个实现ID允许软件探测SBI实现的异常。
 * 每个SBI的实现ID都是不一样的:0,BBL; 1,OpenSBI; 2,Xvisor; 3:KVM。
 */
struct sbiret sbi_get_impl_id(void);
```



##### sbi_get_impl_version

```c
/* 函数ID:2
 * 返回当前SBI的实现版本。版本号是特定于SBI实现的。
 *
 */
struct sbiret sbi_get_impl_version(void);
```



##### sbi_probe_extension

```c
/* 函数ID:3
 * 返回0则扩展ID不可用，返回非0则扩展ID可用。
 *
 */
struct sbiret sbi_probe_extension(long extension_id);
```



##### sbi_get_mvendorid

```c
/* 函数ID:4
 * 返回CSR mvendorid的合法值，0总是合法的。
 *
 */
struct sbiret sbi_get_mvendorid(void);
```



##### sbi_get_marchid

```c
/* 函数ID:5
 * 返回CSR marchid的合法值，0总是合法的。
 *
 */
struct sbiret sbi_get_marchid(void);
```



##### sbi_get_mimpid

```c
/* 函数ID:6
 * 返回CSR mimpid的合法值，0总是合法的。
 *
 */
struct sbiret sbi_get_mimpid(void);
```



#### 遗留SBI扩展

遗留SBI扩展忽略了函数ID字段(为0)，而是被编码为多个扩展ID(其中0x09-0x0F被保留)。必须直接探测每个扩展ID。

遗留SBI扩展被弃用，可以用随后列出的扩展来代替。但控制台SBI函数`sbi_console_getchar()`和`sbi_console_putchar()`将被弃用且没有替代器。

##### sbi_set_timer

```c
/* 扩展ID:0x00
 * 在stime_value时间后触发下次事件，并清除挂起的计时器中断位。
 */
void sbi_set_timer(uint64_t stime_value)
```

如果管理员特权级希望在不调度下次计时器事件的情况下清除计时器中断，它可以请求一个无限远的计时器中断(如：uint64_t - 1)，或通过清除`sie.STIE`来屏蔽计时器中断。

##### sbi_console_putchar

```c
/* 扩展ID:0x01
 * 把ch代表的数据写入控制台。
 */
void sbi_console_putchar(int ch)
```

如果有挂起的字符等待传输，或接收端没有准备好接收字节，此SBI调用会阻塞。如果控制台不存在，字符会被丢弃。

##### sbi_console_getchar

```c
/* 扩展ID:0x02
 * 从调试控制台读入一个字节。
 * 返回值：成功则返回读入的字节，错误则返回-1。
 */
int sbi_console_getchar(void)
```



##### sbi_clear_ipi

```c
/* 扩展ID:0x03
 * 清除挂起的IPI。只有调用此函数的hart的IPI才会被清除。
 */
void sbi_clear_ipi(void)
```



##### sbi_send_ipi

```c
/* 扩展ID:0x04
 * 发送处理器间中断到hart_mask指定的所有核。hart_mask是一个指向harts位向量的虚拟地址。
 * 处理器间中断在接收hart表现为管理员级软件中断。
 */
void sbi_send_ipi(const unsigned long *hart_mask)
```

TODO：不理解harts位向量的结构。harts数除以unsigned long的大小？

##### sbi_remote_fence_i

```c
/* 扩展ID:0x05
 * 指示远程harts执行FENCE.I指令。
 */
void sbi_remote_fence_i(const unsigned long *hart_mask)
```



##### sbi_remote_sfence_vma

```c
/* 扩展ID:0x06
 * 指示远程harts执行一个或多个SFENCE.VMA指令
 * start:起始虚拟地址
 * size:虚拟地址大小
 */
void sbi_remote_sfence_vma(const unsigned long *hart_mask,
                           unsigned long start,
                           unsigned long size)
```



##### sbi_remote_sfence_vma_asid

```c
/* 扩展ID:0x07
 * 指示远程harts执行一个或多个SFENCE.VMA指令,仅覆盖给定的ASID。
 * start:起始虚拟地址
 * size:虚拟地址大小
 */
void sbi_remote_sfence_vma_asid(const unsigned long *hart_mask,
                                unsigned long start,
                                unsigned long size,
                                unsigned long asid)
```

ASID(Adress Space ID)：用于支持process-specific的TLB。

##### sbi_shutdown

```c
/* 扩展ID:0x08
 * 把所有hart都置于关闭状态。此SBI调用不返回。
 */
void sbi_shutdown(void)
```


#### 计时器扩展

##### sbi_set_timer

```c
/* 扩展ID:0x54494D45，函数ID:0。用于取代遗留的计时器扩展0x00。
 *
 */
struct sbiret sbi_set_timer(uint64_t stime_value) 
```



#### IPI扩展

##### sbi_send_ipi

```c
/* 扩展ID:0x735049，函数ID:0。用于取代遗留扩展0x03。
 * 返回SBI_SUCCESS，表明IPI成功发送到目标harts。
 */
struct sbiret sbi_send_ipi(unsigned long hart_mask, unsigned long hart_mask_base)
```



#### RFENCE扩展

远程fence函数如果要进行完整的TLB刷新，则

- `start_addr`和`size`都是0
- `size`等于2^(XLEN-1)

##### sbi_remote_fence_i

```c
/* 扩展ID:0x52464E43，函数ID:0
 * 
 */
struct sbiret sbi_remote_fence_i(unsigned long hart_mask, unsigned long hart_mask_base)
```



##### sbi_remote_sfence_vma

```c
/* 扩展ID:0x52464E43，函数ID:1
 * 返回SBI_ERR_INVALID_ADDRESS，表明start_addr或size无效。
 */
struct sbiret sbi_remote_sfence_vma(unsigned long hart_mask,
				    unsigned long hart_mask_base,
				    unsigned long start_addr,
				    unsigned long size)
```



##### sbi_remote_sfence_vma_asid

```c
/* 扩展ID:0x52464E43，函数ID:2
 * 返回SBI_ERR_INVALID_ADDRESS，表明start_addr或size无效。
 */
struct sbiret sbi_remote_sfence_vma_asid(unsigned long hart_mask,
				unsigned long hart_mask_base,
                unsigned long start_addr, unsigned long size,
                unsigned long asid)
```



##### sbi_remote_hfence_gvma_vmid

```c
/* 扩展ID:0x52464E43，函数ID:3
 * 指示远程harts执行一个或多个HFENCE.GVMA指令。
 * 覆盖的是客户机的物理地址，仅覆盖给定的VMID。
 * 此函数仅对实现了hypervisor扩展的harts的函数调用有效。
 */
struct sbiret sbi_remote_hfence_gvma_vmid(unsigned long hart_mask,
				unsigned long hart_mask_base,
                unsigned long start_addr, unsigned long size,
                unsigned long vmid)
```



##### sbi_remote_hfence_gvma

```c
/* 扩展ID:0x52464E43，函数ID:4
 * 指示远程harts执行一个或多个HFENCE.GVMA指令。
 * 此函数仅对实现了hypervisor扩展的harts的函数调用有效。
 */
struct sbiret sbi_remote_hfence_gvma(unsigned long hart_mask,
				unsigned long hart_mask_base,
                unsigned long start_addr, unsigned long size)
```



##### sbi_remote_hfence_vvma_asid

```c
/* 扩展ID:0x52464E43，函数ID:5
 * 指示远程harts执行一个或多个HFENCE.VVMA指令。
 * 此函数仅对实现了hypervisor扩展的harts的函数调用有效。
 */
struct sbiret sbi_remote_hfence_vvma_asid(unsigned long hart_mask,
				unsigned long hart_mask_base,
                unsigned long start_addr, unsigned long size,
                unsigned long asid)
```



##### sbi_remote_hfence_vvma

```c
/* 扩展ID:0x52464E43，函数ID:6
 * 指示远程harts执行一个或多个HFENCE.VVMA指令。
 * 此函数仅对实现了hypervisor扩展的harts的函数调用有效。
 */
struct sbiret sbi_remote_hfence_vvma(unsigned long hart_mask,
				unsigned long hart_mask_base,
                unsigned long start_addr, unsigned long size)
```



#### 核状态管理扩展

##### sbi_hart_start

```c
/* 扩展ID:0x48534D，函数ID:0
 * 通知SBI实现，管理员特权级希望给定的hart开始执行。
 * 这个调用是异步的：函数的返回了，并不意味着给定的hart已经开始执行了。
 * start_addr - 一个运行时指定(runtime-specified)的物理地址，hart恢复执行的地址。
 * priv - 从start_addr开始执行时，a1将包含这个值。
 */
struct sbiret sbi_hart_start(unsigned long hartid, unsigned long start_addr, unsigned long priv)
```



##### sbi_hart_stop

```c
/* 扩展ID:0x48534D，函数ID:1
 * 将调用hart的所有权返回给SBI实现。
 * 必须在管理员和用户中断禁用的情况下调用此函数。
 */
struct sbiret sbi_hart_stop()
```



##### sbi_hart_get_status

```c
/* 扩展ID:0x48534D，函数ID:2
 * 返回hartid的当前状态。
 * 返回值：0 已经开始，1 结束，2 开始请求被挂起，3 结束请求被挂起。
 */
struct sbiret sbi_hart_status(unsigned long hartid)
```



#### 实验性SBI扩展空间

#### 制造商SBI扩展空间

#### 特定固件代码SBI扩展空间

