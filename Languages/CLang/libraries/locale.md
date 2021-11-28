#### 概述

语言环境管理

locale指语言环境

#### 相关函数

```c
/*
 * 设置或检索locale
 * category：要修改的locale的种类
   * LC_CTYPE:字符的类别
 * locale：locale的值
   * “C”或“POSIX”代表可移植的locale，所有的合规系统中都应有它们
 */
char *setlocale(int category, const char *locale);
```

