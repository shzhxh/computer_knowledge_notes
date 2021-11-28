#### 概述

对宽字符的操作，宽字符就是由多个字节组成的字符

#### 相关函数

```c
/* 把1个字节转换为宽字符 */
wint_t btowc(int c);

/*
 * 把多字节序列转换为宽字符
 * pwc：宽字符
 * S：多字节序列
 * n：要转换的字节数
 * ps：记录移动状态
 */
size_t mbrtowc(wchar_t *pwc, const char *s, size_t n, mbstate_t *ps);

/* 把多字节字符串src转换为宽字符组成的字符串dest,len是转换的字符数量，ps是移位状态 */
size_t mbsrtowcs(wchar_t *dest, const char **src, size_t len, mbstate_t *ps);

/* 把一个宽字符转化成多字符序列
 * pwc：多字符序列的指针
 * wc：要转化的宽字符
 * ps：移位状态
 */
size_t wcrtomb(char *s, wchar_t wc, mbstate_t *ps);

/* 在由宽字符组成的字符串wcs内搜索宽字符wc
 * 返回值：如果找到对应字符返回其指针，否则返回NULL。
 */
wchar_t *wcschr(const wchar_t *wcs, wchar_t wc);

/* 把由宽字符组成的字符串src转换到多字节字符串dest，len是转换的字符数量，ps是移位状态 */
size_t wcsrtombs(char *dest, const wchar_t **src, size_t len, mbstate_t *ps);

/*  */
long wcstol (const wchar_t *__restrict, wchar_t **__restrict, int);
unsigned long wcstoul (const wchar_t *__restrict, wchar_t **__restrict, int);

/* 等价于宽字节版的printf()函数族的功能 */
int wprintf(const wchar_t *format, ...);
int fwprintf(FILE *stream, const wchar_t *format, ...);
int swprintf(wchar_t *wcs, size_t maxlen, const wchar_t *format, ...);
int vwprintf(const wchar_t *format, va_list args);
int vfwprintf(FILE *stream, const wchar_t *format, va_list args);
int vswprintf(wchar_t *wcs, size_t maxlen, const wchar_t *format, va_list args);

int wscanf (const wchar_t *__restrict, ...);
int fwscanf (FILE *__restrict, const wchar_t *__restrict, ...);
int swscanf (const wchar_t *__restrict, const wchar_t *__restrict, ...);
```

