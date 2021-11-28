### 简介

关于可变参数的处理，详见`stdarg.h`（注：此文件不属于glibc标准库，但属于musl-dev库）。

通过`man stdarg`获取帮助

### 数据结构

#### va_list

是一个变量类型，它将依次指向每个参数。

### 宏

#### va_start

`void va_start(va_list ap, last);`

在访问可变参数前，必须用va_start初始化变量ap。last是可变参数之前最后一个参数的名称。

#### va_arg

`type va_arg(va_list ap, type);`

将返回下一个参数。

#### va_end

`void va_end(va_list ap);`

参数处理完毕后，必须调用va_end一次。

#### va_copy

`void va_copy(va_list dest, va_list src);`

将可变参数表src复制到dest。