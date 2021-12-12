#### 简介

string.h定义了两组函数，一组以str开头，一组以mem开头。用于管理字符串。

#### 定位字符串中的字符

```c
/* 返回字符c在字符串s中第一次匹配时的指针，若不能匹配则返回NULL。 */
char *strchr(const char *s, int c);
  
/* 返回字符c在字符串s中最后一次匹配时的指针，若不能匹配则返回NULL。 */
char *strrchr(const char *s, int c);
  
/* 类似于strchr，不同点在于如果没有在s里匹配到c，不是返回的NULL，而是返回在s后面那个null字节(应该指的是字符串后面那个`\0`字节)的指针。 */
char *strchrnul(const char *s, intc);
```

#### 连接两个字符串

```c
/*在字符串dest后面附加src字符串。要求两个字符串不可重叠，且dest要有足够的空间。*/
char *strcat(char *dest, const char *src);

/* 类似于strcat，不同点在于最多只使用src里的n个字节，如果src包含n个或更多的字符，它也不必使用null作为终结符。 */
char *strncat(char *dest, const char *src, size_t n);
```

#### 复制字符串

```c
/* 复制字符串s，并返回其指针。新字符串的内存是通过malloc()获得的。*/
char *strdup(const char *s);
/* 复制字符串s，并返回其指针，但最多只复制n个字节。新字符串的内存是通过malloc()获得的。*/
char *strndup(const char *s, size_t n);
/* 复制字符串s，并返回其指针。新字符串的内存是通过alloca()获得的。*/
char *strdupa(const char *s);
/* 复制字符串s，并返回其指针，但最多只复制n个字节。新字符串的内存是通过alloca()获得的。*/
char *strndupa(const char *s, size_t n);
```

#### 复制内存区域

```c
/* 从src向dest复制n个字节的内容。要求src和dest两个内存区域不可重叠。其返回值是对dest的指针。 */
void *memcpy(void *dest, cosnt void *src, size_t n);
/* 在内存s上填充n个字节，字节内容为c */
void *memset(void *s, int c, size_t n);
bcopy
/*  */
memccpy
/*  */
memmove
/*  */
mempcpy
/* 复制字符串
 * 返回值：到dest的指针。
 */
char *strcpy(char *dest, const char *src);
/* 复制指定长度字符串
 * 返回值：到dest的指针。
 */
char *strncpy(char *dest, const char *src, size_t n);
/*  */
wmemcpy
```

#### 其它

```c
/* 计算字符串s里有多少个字符属于accept */
size_t strspn(const char *s, const char *accept);
/* 计算字符串s里有多少个字符不属于reject */
size_t strcspn(const char *s, const char *reject);
/* 查找长度为needlelen的子字符串needle在长度为haystacklen的内存区域haystack首先出现的地址 */
void *memmem(const void *haystack, size_t haystacklen, const void *needle, size_t needlelen);
/* 查找字符串needle在字符串haystack中首次出现的位置 */
char *strstr(const char *haystack, const char *needle);
/*
 * 作用：分割字符串stringp，分割符来自于delim
 * stringp：被分割的字符串。如为NULL表示分割完成，不作任何事而直接返回NULL。
 * delim：由分割符组成的字符串。
 */
char *strsep(char **stringp, const char *delim);
/* 返回字符串的长度 */
extern size_t strlen (const char *__s)
     __THROW __attribute_pure__ __nonnull ((1));
/* 比较两个字符串  */
extern int strcmp (const char *__s1, const char *__s2)
     __THROW __attribute_pure__ __nonnull ((1, 2));
/* 比较两个字符串里的n个字符  */
extern int strncmp (const char *__s1, const char *__s2, size_t __n)
     __THROW __attribute_pure__ __nonnull ((1, 2));
/* 依据delim中的字符分割字符串str */
char *strtok(char *str, const char *delim);

char *strtok_r(char *str, const char *delim, char **saveptr);
/* 以目标字符串的所有字符作为集合，在当前字符串查找该集合的任一元素 */
char *strpbrk(const char *s, const char *accept);
```

