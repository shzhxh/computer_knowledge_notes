string.h定义了两组函数，一组以str开头，一组以mem开头

#### 定位字符串中的字符

- char *strchr(const char *s, int c);

  返回字符c在字符串s中第一次匹配时的指针，若不能匹配则返回NULL。

- char *strrchr(const char *s, int c);

  返回字符c在字符串s中最后一次匹配时的指针，若不能匹配则返回NULL。

- char *strchrnul(const char *s, intc);

  类似于strchr，不同点在于如果没有在s里匹配到c，不是返回的NULL，而是返回在s后面那个null字节(应该指的是字符串后面那个`\0`字节)的指针。

#### 连接两个字符串

- char *strcat(char *dest, const char *src);

  在字符串dest后面附加src字符串。要求两个字符串不可重叠，且dest要有足够的空间。

- char *strncat(char *dest, const char *src, size_t n);

  类似于strcat，不同点在于最多只使用src里的n个字节，如果src包含n个或更多的字符，它也不必使用null作为终结符。

#### 复制字符串

```
char *strdup(const char *s);	//复制字符串s，并返回其指针。
char *strndup();
char *strdupa();
char *strndupa();
```



#### 复制内存区域

- void *memcpy(void *dest, cosnt void *src, size_t n);

  从src向dest复制n个字节的内容。要求src和dest两个内存区域不可重叠。其返回值是对dest的指针。

- bcopy
- memccpy
- memmove
- mempcpy
- strcpy
- strncpy
- wmemcpy