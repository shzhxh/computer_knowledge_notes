#### 可变参数宏

```c
#define dgbmsg(fmt, ...) printf(fmt, __VA_ARGS__)
// ...代表一个可变参数表， 保留名__VA_ARGS__把参数传递给宏
// ##的作用是：如果可变参数为空或被忽略，预处理器会去掉它前面的逗号
```

#### 查看GCC预定义的宏

`gcc -E -dM - </dev/null`

#### \__attribute__

\__attribute__关键字用于定义变量或结构体的属性。

- section子项的用法

  `__attribute__((section("section_name")))`，其作用是将函数或数据放入名为setion_name的段。