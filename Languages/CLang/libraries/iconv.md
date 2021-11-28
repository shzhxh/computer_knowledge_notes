#### 概述

用于将一种字符集转换成另一种字符集。

iconv的意思可能是Identifier for conversion（用于转换的标识符）。

```c
/* 实现字符集的转换
 * cd：转换描述符，由iconv_open函数生成
 * inbuf：输入序列的指针
 * inbytesleft：输入序列的字节数
 * outbuf：输出序列的指针
 * outbytesleft：输出序列的字节数
 */
size_t iconv(iconv_t cd, char **inbuf, size_t *inbytesleft, char **outbuf, size_t *outbytesleft);

/* 分配一个转化描述符，使得可以把字符序列从编码fromcode转化为编码tocode */
iconv_t iconv_open(const char *tocode, const char *fromcode);

/* 释放用于字符集转换的描述符cd */
int iconv_close(iconv_t cd);
```

