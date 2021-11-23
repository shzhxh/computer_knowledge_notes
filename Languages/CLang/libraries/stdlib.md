实用函数

#### 数组排序与字符串转换

```c
/* 宏定义 */
// 当前local里多字节字符的最大长度
#define	MB_CUR_MAX	(__ctype_get_mb_cur_max ())

/* 给数组排序
 * base：数组的指针
 * nmemb：数组元素数量
 * size：数组的大小
 * compar：元素比较函数，它是数组排序的依据
 */
void qsort(void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *));
void qsort_r(void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *, void *), void *arg);

/* 把ASCII字符串转换为浮点数 */
double strtod(const char *nptr, char **endptr);
float strtof(const char *nptr, char **endptr);
long double strtold(const char *nptr, char **endptr);

/*
 * 把<*nptr>指向的字符串转化为一个长整型的值。
 * 参数:base，字符串所代表的整型数的基数。
 * 参数：endptr，当函数返回时，若<*nptr>指向的字符串可以完全被翻译成一个数字，则此值被设置成NULL;当<*nptr>指向的字符串不能完全被翻译成一个数字，则此值会被设置成指向字符串里不能被翻译的部分；当<*nptr>指向的字符串完全不能被翻译成一个数字，此值会指向整个字符串。
 */
long strtol (const char *__restrict, char **__restrict, int);
unsigned long strtoul (const char *__restrict, char **__restrict, int);
long long strtoll (const char *__restrict, char **__restrict, int);
unsigned long long strtoull (const char *__restrict, char **__restrict, int);
```

#### 环境变量

```c
/* 清除环境 */
int clearenv(void);
/* 改变或添加环境变量 */
int putenv(char *string);
/* 获取环境变量 */
char *getenv(const char *name);
/* 改变或添加环境变量 */
int setenv(const char *name, const char *value, int overwrite);
/* 删除环境变量 */
int unsetenv(const char *name);
```

#### 随机数

```c
/* 生成一个伪随机数 */
long int random(void);

/* 为random()函数设置seed，因为random()产生的伪随机数取决于seed */
void srandom(unsigned int seed);

/* 初始化状态数组state给random()函数用 */
char *initstate(unsigned int seed, char *state, size_t n);

/* 改变random()函数所使用的状态数组state */
char *setstate(char *state);
```

#### 分配内存

```c
void *malloc(size_t size);	// 分配大小为size字节的内存，返回其指针。注：分配到的内存未初始化。
void free(void *ptr);	// 释放ptr指向的内存。注：prt必须是malloc()，calloc()，或realloc()的返回值。
void *calloc(size_t nmemb, size_t size);	// 为数组分配内存，nmemb是数组中元素数，size是每个元素的大小。注：分配到的内存会初始化为0.
void *realloc(void *ptr, size_t size);
void *reallocarray(void *ptr, size_t nmemb, size_t size);
```

