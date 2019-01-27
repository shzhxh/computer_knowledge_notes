#### 基本概念

##### 流

流是数据的源或目的地，它实际上对应着的是一个设备，比如磁盘。程序开始执行时，stdin, stdout, stderr这3个流已经打开。

####  格式化输出的函数

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

```

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

#### 格式化输入函数

```c
int fscanf(FILE *stream, const char *format, ...);	// 依据格式串format从*stream中读取输入，把值赋给后续各个参数
int scanf(const char *format, ...);	// 等价于fscanf(stdin, *format, ...)
int sscanf;
```

#### 字符输入输出函数

```c
int fgetc();
int fgets();
int fputc();
int fputs();
```



#### 文件操作函数

```c
FILE *fopen(const char *path, const char *mode);
	// 打开文件名为path的文件，并关联到一个流。参数mode指打开的模式。
FILE *fdopen(int fd, const char *mode);
FILE *freopen(const char *path, const char *mode, FILE *stream);
int fclose(FILE *stream);	// 关闭流
```

##### 解读mode

- r：以读的方式打开文件，流指向文件开头。
- r+：以读写的方式打开，流指向文件开头。
- w：将文件截断为0长度，或创建可写的文件，流指向文件开头。
- w+：以读写的方式打开，文件不存在则会创建，存在则截断。流指向文件开头。
- a：以附加的方式打开，文件不存在则会创建，流指向文件末尾。
- a+：以附加且可读的方式打开，文件不存在则会创建，但读是从文件开头，写是从文件末尾。
- 还可以附加`b`表示是二进制，但这对POSIX兼容系统来说没有效果，而只是为了与C89兼容。

#### 直接输入输出函数

```c
size_t fread(void *ptr, size_t size, size_t nobj, FILE *stream);	// 从流stream中最多读取nobj个长度为size的对象，保存到ptr指向的数组中。返回读取的对象数，此值可能小于nobj。
size_t fwrite(void *ptr, size_t size, size_t nobj, FILE *stream);	// 从ptr指向的数组中读取nobj个长度为size的对象，输出到流stream中。返回输出的对象数。
```



#### 文件定位函数

```c
int fseek(FILE *stream, long offset, int origin);	// 设置流stream的位置为从origin开始的第offset个字符，后续的读写操作将从新位置开始。origin的值可以是SEEK_SET(文件开始)，SEEK_CUR(当前位置)，SEEK_END(文件末尾)
long ftell(FILE *stream);	// 返回stream流当前的位置
```

#### 错误处理函数

```c
void perror(const char *s);	// 打印字符串s与错误信息
int feof(FILE *stream);	// 测试流stream的文件结束指示符，如果设置了则返回非0。
```

