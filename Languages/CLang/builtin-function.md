#### 用于原子内存访问的内建函数

```
__sync_synchronize(...)		// 产生完整的内存屏障(issues a full memory barrier)
```



#### 其它内建函数

```c
int __builtin_constant_p (exp);	// 用来确定exp是否在编译时是常量，是则返回1,否则返回0.

long __builtin_expect(long exp,long c); // 为编译器提供分支预测的信息,意为期望exp的值与c的值相等。也可以使用参数'-fprofile-arcs'来让gcc收集分支走向的信息，然而对于很多程序来说这样的信息是很难收集的。exp为一个整型表达式，c必须是一个编译期的常量，返回值为第一个参数exp的值。

void __builtin_trap(void);	// 此函数将使程序异常的退出。GCC通过宿主机的相关机制或调用abort实现此函数。
```

#### 参考资料

- [Extensions to the C Language Family](https://gcc.gnu.org/onlinedocs/gcc-4.4.5/gcc/C-Extensions.html#C-Extensions)