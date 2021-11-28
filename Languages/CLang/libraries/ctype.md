#### 概述

声明了一些测试字符类别的函数。

```c
int isalnum(int c);	// 检查是否为字母和数字
int isalpha(int c);	// 检查是否为字母字符
int iscntrl(int c);	// 检查是否为控制字符
int isdigit(int c);	// 检查是否为数字
int isgraph(int c);	// 检查是否是图形字符，等效于 isalnum() | ispunct()
int islower(int c);	// 检查是否为小写字母字符
int isprint(int c);	// 检查是否是可打印字符
int ispunct(int c);	// 检查是否为标点符号
int isspace(int c);	// 检查是否为空格类型字符
int isupper(int c);	// 检查是否为大写字母字符
int isxdigit(int c);	// 检查是否为十六进制数字表示的有效字符

int isascii(int c);
int isblank(int c);

int isalnum_l(int c, locale_t locale);
int isalpha_l(int c, locale_t locale);
int isblank_l(int c, locale_t locale);
int iscntrl_l(int c, locale_t locale);
int isdigit_l(int c, locale_t locale);
int isgraph_l(int c, locale_t locale);
int islower_l(int c, locale_t locale);
int isprint_l(int c, locale_t locale);
int ispunct_l(int c, locale_t locale);
int isspace_l(int c, locale_t locale);
int isupper_l(int c, locale_t locale);
int isxdigit_l(int c, locale_t locale);

int isascii_l(int c, locale_t locale);
```

