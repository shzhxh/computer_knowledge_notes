### 简介

对标准C库的测试

### wchar.h

#### clocale_mbfuncs

简介：把locale设置为C，然后测试wchar.h库里一些多字节字符串转换函数的功能。

1. 设置locale为C。
2. 在一个for循环里，使用`mbrtowc`把ASCII码转化为宽字符，使用`btowc`把对应宽字符保存在宽字符数组map里。
3. 在一个for循环里，检查map里的元素都不是WEOF，且与对应的ASCII码相等。
4. 在一个for循环里，使用`wcrtomb`把非ASCII码的宽字符保存在多字符序列s里。
5. 使用`wcsrtombs`把map里的宽字符组成的字符串保存进多字符序列s里。
6. 使用`mbsrtowcs`把多字符序列s保存进宽字符组成的字符串wtmp里。
7. 在一个for循环里，检查128~255的宽字符编码，要求非字母数字，非大小写字母，非blank字符，非控制字符，非数字，不可打印或是space字符，非小字字母，不可打印，不是标点符号，不是space字符，不是大写字符，不是16进制字符。

#### wcstol

简介：测试`wcstol(), wcstoul()`两个函数的功能。

#### swprintf

简介：测试`swprintf()`的功能。

#### mbc

简介：测试`mbrtowc, mbsrtowcs`两个函数的功能。

#### wcsstr

简介：测试`wcsstr()`的功能。

#### fwscanf

简介：测试`fwscanf()`的功能。

### string.h

#### string_strchr

简介：使用自定义的N(s,c)和T(s,c,n)两个宏，测试`strchr()`函数的功能。

N(s, c)是把字符串s复制进buf里8个不同的位置，且复制进的字符串不能包含字符c。

T(s,c,n)是把字符串s复制进buf里8个不同的位置，且复制进的字符串第n位是字符c。

1. 在for循环里把a[]设置为包含ASCII码前128个字符的数组。
2. 在for循环里把s[]设置为包含ASCII码256个字符的数组。
3. 使用N(s,c)，设定一些构造好的s，确保`strchr()`搜索不到c.
4. 使用T(s,c,n)，设定一些构造好的s，确保`strchr()`可以在位置n搜索到c。

#### string

简介：使用TEST(r,f,x,m)和TEST_S(s,x,m)两个宏，测试`strcpy(), memset, strncpy, memcmp, strncmp, strncat, strchr, strcspn, strpbrk, strtok, strlcpy, strlcat`函数的功能。

TEST(r,f,x,m)，如果函数f的返回值不等于x，则打印m。

TEST_S(s,x,m)，如果字符串s的值不等于x，则打印m。

#### string_memcpy

简介：使用自定义函数`test_align()`测试`memcpy()`的功能。

#### string_memset

简介：使用两个自定义函数`test_align()`和`test_value()`测试`memset()`的功能。

#### string_strcspn

简介：测试`strspn(), strcspn()`两个函数的功能。

#### string_memmem

简介：测试`memmem()`函数的功能。

#### string_strstr

简介：测试`strstr()`函数的功能。

### libgen.h

#### dirname

简介：使用自定义宏T(path, want)测试`dirname()`的功能。

T(path, want)，获取path里的目录部分，则打印出错误信息。

#### basename

简介：测试`basename()`函数。

### stdlib.h

#### qsort

简介：测试`qsort`的功能。

#### strtof

简介：测试`strtof`的功能。

#### strtod_simple

简介：测试`strtod`的功能。

#### strtod

简介：测试`strtod`的功能。

#### strtol

简介：测试`strtol`的功能。

#### strtold

简介：测试`strtold`的功能。

#### env

简介：测试与环境变量有关的函数`putenv, getenv, setenv, unsetenv, clearenv`。

#### random

简介：测试与随机数有关的函数`random, srandom, initstate, setstate`。

#### sscanf

简介：测试`sscanf()`函数。

### iconv.h

#### iconv_open

简介：测试`iconv(), iconv_open(), iconv_close()`的功能。

### sys/stat.h

#### utime

简介：测试`utimensat(), futimens()`的功能。

#### stat

简介：测试`stat(), fstat()`的功能。

### arpa/inet.h

#### inet_pton

简介：测试ip地址格式转换函数`inet_pton`等的功能。

### pthread.h

#### pthread_mutex

简介：使用自定义函数`test_relock(), test_unlock(), test_unlock_other()`测试`pthread_mutex*`函数相关的功能。

#### pthread_mutex_pi

简介：使用自定义函数`test_relock(), test_unlock(), test_unlock_other()`测试`pthread_mutexattr_*`函数相关的功能。

#### tls_init

简介：测试`pthread_create(), pthread_join()`的功能。

#### tls_local_exec

简介：测试`pthread_create(), pthread_join()`的功能。

#### pthread_tsd

简介：测试tsd相关函数`pthread_key_create, pthread_key_delete, pthread_getsecific, pthread_setspecific`。

#### pthread_cancel

简介：测试`pthread_cancel()`函数。

#### pthread_cond

简介：测试`pthread_cond_*`等函数的功能。

### crypt.h

#### crypt

简介：测试`crypt()`的功能。

### stdio.h

#### fscanf

简介：测试`fscanf()`的功能。

#### memstream

简介：测试`open_memstream(), fmemopen()`的功能。

#### sscanf_long

简介：测试`sscanf()`的功能。

#### snprintf

简介：测试`snprintf()`的功能。

#### fdopen

简介：测试`fdopen()`的功能。

### time.h

#### clock_gettime

简介：测试`clock_gettime()`的功能。

#### strftime

简介：测试`strftime()`的功能。

#### time

简介：测试`mktime, gmtime`两个函数。

#### ungetc

简介：测试`ungetc()`函数。

### search.h

#### search_insque

简介：测试`insque(), remque()`的功能。

#### search_hsearch

简介：测试`hsearch()`的功能。

#### search_lsearch

简介：测试`lsearch()`的功能。

#### search_tsearch

简介：测试`tsearch()`的功能。

### semaphore.h

#### sem_init

简介：测试关于信号量的函数`sem_*`的功能。

### fnmatch.h

#### fnmatch

简介：测试`fnmatch()`的功能。

### setjmp.h

#### setjmp

测试`setjmp, longjmp`两个函数。

### 非函数测试

#### udiv

简介：测试除法和取余两种运算。

#### argv

简介：测试main函数的参数。

#### tls_align

简介：测试内存地址对齐。

