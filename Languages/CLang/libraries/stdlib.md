实用函数

```
/*
 * 把<*nptr>指向的字符串转化为一个无符号长整型的值。
 * 参数:base，字符串所代表的整型数的基数。
 * 参数：endptr，当函数返回时，若<*nptr>指向的字符串可以完全被翻译成一个数字，则此值被设置成NULL;当<*nptr>指向的字符串不能完全被翻译成一个数字，则此值会被设置成指向字符串里不能被翻译的部分；当<*nptr>指向的字符串完全不能被翻译成一个数字，此值会指向整个字符串。
 */
unsigned long int strtoul(const char *nptr, char **endptr, int base);
```

