#### 概述

管理输入输出

#### 基本概念

- **流**是数据的源或目的地，它实际上对应着的是一个设备，比如磁盘。程序开始执行时，stdin, stdout, stderr这3个流已经打开。

#### 文件操作

```c
/*
  ##### 解读mode
- r：以读的方式打开文件，流指向文件开头。
- r+：以读写的方式打开，流指向文件开头。
- w：将文件截断为0长度，或创建可写的文件，流指向文件开头。
- w+：以读写的方式打开，文件不存在则会创建，存在则截断。流指向文件开头。
- a：以附加的方式打开，文件不存在则会创建，流指向文件末尾。
- a+：以附加且可读的方式打开，文件不存在则会创建，但读是从文件开头，写是从文件末尾。
- 还可以附加`b`表示是二进制，但这对POSIX兼容系统来说没有效果，而只是为了与C89兼容。
 */
/* 打开文件名为path的文件，并关联到一个流。参数mode指打开的模式。 */
FILE *fopen(const char *path, const char *mode);
/* 把文件描述符关联到一个流 */
FILE *fdopen(int fd, const char *mode);
FILE *freopen(const char *path, const char *mode, FILE *stream);
int fclose(FILE *stream);	// 关闭流


/*
#### 流缓冲区操作

有三种缓冲的方式：

- unbuffered：信息直接写到文件或终端上。stderr默认是无缓冲的。
- block buffered：信息以块的形式保存和写入。通常所有的文件都是块缓冲的。
- line buffered：信息以行的形式保存。当流指向终端(如stdout)则是行缓冲的。
 */
void setbuf(FILE *stream, char *buf);

void setbuffer(FILE *stream, char *buf, size_t size);

void setlinebuf(FILE *stream);
/* 将缓冲区与流相关联，本函数应该仅可以在打开一个流且没有进行其它操作前执行。
 * buf：用于取代当前缓冲区的新缓冲区指针。如为NULL则会在下次读或写的时候分配缓冲区。
 * mode：_IONBF，无缓冲;_IOLBF行缓冲；_IOFBF全缓冲。
 * size:缓冲区大小。
 */
int setvbuf(FILE *stream, char *buf, int mode, size_t size);
```



####  格式化输出

```c
int fprintf(FILE *stream, const char *format, ...)
  // fprintf函数将带有格式标记的字符串format写到stream流中。返回值是实际写入的字符数，若出错则返回一个负值。
- printf(...)等价于fprintf(stdout, ...)
- dprintf是将输出流设定为文件描述符fd。
- sprintf(char *s,const char *format, ...)是将format写入到字符串s中。
- snprintf类似于sprintf，但给定了字符串的长度。
- vprintf等价于printf,但用arg代替了可变参数表，arg由宏va_start或va_arg初始化。
- vfprintf等价于fprintf，但用arg代替了可变参数表，arg由宏va_start或va_arg初始化。
- vdprintf等价于dprintf，但用arg代替了可变参数表。
- vsprintf等价于sprintf,但用arg代替了可变参数表，arg由宏va_start或va_arg初始化。
- vsnprintf等价于snprintf，但用arg代替了可变参数表。
/*
##### format的结构
格式化字符串format ＝ 普通字符串 ＋ 转换说明
转换说明 ＝ % + 约束部分 + 转换字符

##### format约束部分
	标志：
	  -		// 左对齐
	  +		// 加正负号
	  空格	// 如果第一个字符不是正负号，则在其前面加空格
	  0		// 对于数值转换，当输出长度小于字段宽度时，则在其前面填充0
	  #		// 指定另一种输出形式，o转换则加前缀o，x或X转换加前缀0x或0X，e、E、f、g、G转换，结果有小数时才给出小数点。
	数值(宽度)	// 指定最小字段宽度
	.		// 分隔字段宽度和精度
	数值(精度)	//对于字符串指定最大长度，e、E或f转换指定小数点后数位的长度，g或G转换指定有效位长度，对整型数则指定数位长度。
	h		// 按short或unsigned short输出。
	l		// 按long或unsigned long输出。
	L		// 按long double输出。
	*		// 宽度或精度的任何一个都可以用*指定

##### format转换字符
	d,i		// int,有符号十进制数
	u		// unsigned int,无符号十进制数
	o		// unsigned int,无符号八进制数
	x,X		// unsigned int,无符号十六进制（无前导0x或0X)
	c		// int
	s		// char *,字符串
	f		// double,双精度型
	e,E		// double,双精度型（指数形式）
	g,G		// double,f或e,E类型，取决于限定条件
	p		// void *,指针
	n		// int *,到当前为止输出的字符数
	%		// 不转换,打印%
	
 */
```



#### 格式化输入

```c
int fscanf(FILE *stream, const char *format, ...);	// 依据格式串format从*stream中读取输入，把值赋给后续各个参数
int scanf(const char *format, ...);	// 等价于fscanf(stdin, *format, ...)
/* 从字符串读取格式化输入 */
int sscanf(const char *str, const char *format, ...);
```

#### 字符输入输出

```c
/* 从流中读取下一个字符.
 * 返回值：读取到的字符，或EOF，或error。
 */
int fgetc(FILE *);
/* 从流中读取下一个字符。它可以实现为宏对流进行多次运算。
 * 返回值：读取到的字符，或EOF，或error。
 */
int getc(FILE *);
/* 从stdin中读取下一个字符。它可以实现为宏对流进行多次运算。
 * 返回值：读取到的字符，或EOF，或error。
 */
int getchar(void);
/* 把字符推入到流中 */
int ungetc(int, FILE *);

int fputc(int, FILE *);
int putc(int, FILE *);
int putchar(int);

char *fgets(char *__restrict, int, FILE *__restrict);
int fputs(const char *__restrict, FILE *__restrict);
int puts(const char *);
```

#### 直接输入输出

```c
size_t fread(void *ptr, size_t size, size_t nobj, FILE *stream);	// 从流stream中最多读取nobj个长度为size的对象，保存到ptr指向的数组中。返回读取的对象数，此值可能小于nobj。
size_t fwrite(void *ptr, size_t size, size_t nobj, FILE *stream);	// 从ptr指向的数组中读取nobj个长度为size的对象，输出到流stream中。返回输出的对象数。
```

#### 文件定位

```c
int fseek(FILE *stream, long offset, int origin);	// 设置流stream的位置为从origin开始的第offset个字符，后续的读写操作将从新位置开始。origin的值可以是SEEK_SET(文件开始)，SEEK_CUR(当前位置)，SEEK_END(文件末尾)
long ftell(FILE *stream);	// 返回stream流当前的位置
```

#### 错误处理

```c
void perror(const char *s);	// 打印字符串s与错误信息
int feof(FILE *stream);	// 测试流stream的文件结束指示符，如果设置了则返回非0。
```



#### 其它

```c
/* 打开一个动态内存缓冲区流 */
FILE *open_memstream(char **ptr, size_t *sizeloc);

/* 把内存作为stream打开 */
FILE *fmemopen(void *buf, size_t size, const char *mode);
```

