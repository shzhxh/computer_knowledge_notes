#### 用于原子内存访问的内建函数

```
__sync_synchronize(...)		// 产生完整的内存屏障(issues a full memory barrier)。即，告诉编译器和处理器不要把load和store放在此点之后。(对于自旋锁来说，以保证临界区的内存引用发生在获取锁之后）
__sync_lock_test_and_set(type *ptr, type value, ...)	// 这个内建函数完成的，不是传统的test-and-set操作，而是原子交换操作。它把value写入*ptr，并返回*ptr之前的值。许多平台只对这样的锁提供最低限度的支持，并且不支持完整的交换操作。即，平台支持的有效value可能只有1。实际存储在*ptr里的值取决于具体的实现。这个内建函数不是完整的屏障(full barrier)，而是获取屏障(aquire barrier)。这意味着内建函数之后的引用不能移动到内建函数之前，但之前的内存store可能还不是全局可见的，且之前的内存load可能还没有得到满足。
```



#### 其它内建函数

```c
int __builtin_constant_p (exp);	// 用来确定exp是否在编译时是常量，是则返回1,否则返回0.

long __builtin_expect(long exp,long c); // 为编译器提供分支预测的信息,意为期望exp的值与c的值相等。也可以使用参数'-fprofile-arcs'来让gcc收集分支走向的信息，然而对于很多程序来说这样的信息是很难收集的。exp为一个整型表达式，c必须是一个编译期的常量，返回值为第一个参数exp的值。

void __builtin_trap(void);	// 此函数将使程序异常的退出。GCC通过宿主机的相关机制或调用abort实现此函数。
```

#### 参考资料

- [Extensions to the C Language Family](https://gcc.gnu.org/onlinedocs/gcc-4.4.5/gcc/C-Extensions.html#C-Extensions)