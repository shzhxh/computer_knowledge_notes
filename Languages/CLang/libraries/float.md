#### 浮点数在内存中的存储形式

$$flt=(-1)^{sign}*mantissa*base^{exponent}$$

- flt - 要表示的浮点数
- sign - 0表示flt是正数，1表示flt是负数
- base - 基数
- mantissa - 精度。$1<=mantissa<base$
- exponent - 指数。

#### 宏

有三种浮点数：float，double，long double。

FLT开头的宏描述float类型，DBL开头的宏描述double类型，LDBL开头的宏描述long double类型。

- FLT_RADIX

  代表基数，即base的值。它对所有的浮点类型(float, double, long double)都有效。

- LDBL_MANT_DIG

  代表mantissa的最大位数。

