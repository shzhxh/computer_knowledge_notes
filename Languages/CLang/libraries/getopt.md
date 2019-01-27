getopt.h是用来获取函数参数的。函数getopt只支持短格式，函数getopt_long长、短格式都支持，函数getopt_long_only与getopt_long基本相同，区别在于长选项字串是用"-"开始的，而不是"--"。

#### 变量

```
extern char *optarg;	# 用于getopt和调用者之间通信，当getopt找到带有参数的选项时，参数值放在这里。
extern int optind, opterr, optopt;
  # optind：argv里下一个待处理参数的索引。用于对getopt的重复调用。
  # opterr：调用者在这里存储0以禁止getopt因未识别的选项而打印错误消息。
  # optopt：设置未识别的选项字符。
```

#### getopt

`int getopt(int argc, char * const argv[], const char *optstring);`

作用：解析命令行参数。当重复调用，它会依次返回各个参数里的字符串。当无参数可解析时，返回-1。

argc：参数数量，来自于main函数。

argv：参数数组，来自于main函数。以'-'开头的被认为是一个参数。

optstring：

#### getopt_long

```
extern int getopt_long (int argc, char *const argv[],
			const char *optstring,
		    const struct option *longopts, int *longindex);
```

#### getopt_long_only

```
extern int getopt_long_only (int argc, char *const argv[],
			     const char *optstring,
		         const struct option *longopts, int *longindex);
```

argc, argv：直接从main函数传递而来

shortopts：短选项字符串。需要传递参数时要在后面加冒号。

longopts：结构体的数组，用于存放长选项参数。

longind：长选项在longopts中的索引，用于调试，一般为NULL。
