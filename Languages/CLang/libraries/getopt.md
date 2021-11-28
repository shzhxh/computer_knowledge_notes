#### 概述

获取函数参数。

#### 变量

```c
/* 定义在bits/getopt_core.h */
extern char *optarg;	// 用于getopt和调用者之间通信，当getopt找到带有参数的选项时，参数值放在这里。
extern int optind, opterr, optopt;
  // optind：argv里下一个待处理参数的索引。用于对getopt的重复调用。
  // opterr：调用者在这里存储0以禁止getopt因未识别的选项而打印错误消息。
  // optopt：设置未识别的选项字符。

/* 定义在bits/getopt_ext.h */
struct option{
  const char *name;	// 长选项
  int has_arg;		// 是否需要参数，0不需要，1需要，2均可
  int *flag;		// 控制getopt函数的返回值，为NULL则返回val(此时val即为字段1所对应的短选项)，非空同指向val的变量
  int val;		// 本字段的意义由第3个字段决定，可能代表短选项，或第3个字段指向的变量
};
```

#### getopt

```c
/* 定义在bits/getopt_core.h */
// 作用：解析命令行参数，只支持短选项。
// argc：参数数量，来自于main函数。
// argv：参数数组，来自于main函数。以'-'开头的被认为是一个参数。
// optstring：
// 返回值：当重复调用，它会依次返回各个参数里的字符串。当无参数可解析时，返回-1。
int getopt(int argc, char * const argv[], const char *optstring);
```

#### getopt_long

```c
/* 定义在bits/getopt_ext.h */
// 作用：解析命令行参数，支持--开头的长选项和-开头的短选项。
// argc, argv：直接从main函数传递而来
// shortopts：短选项字符串。需要传递参数时要在后面加冒号。
// longopts：结构体option的数组，用于存放长选项参数。
// longind：长选项在longopts中的索引，用于调试，一般为NULL。
// 返回值：如option.flag==NULL，则返回值为option.val。如返回-1表示处理完毕。
extern int getopt_long (int argc, char *const argv[],
			const char *optstring,
		    const struct option *longopts, int *longindex);
```

#### getopt_long_only

```c
/* 定义在bits/getopt_ext.h */
// 作用：与getopt_long基本相同，区别在于长选项字串是用"-"开始的，而不是"--"。
// argc, argv：直接从main函数传递而来
// shortopts：短选项字符串。需要传递参数时要在后面加冒号。
// longopts：结构体option的数组，用于存放长选项参数。
// longind：长选项在longopts中的索引，用于调试，一般为NULL。
// 返回值：如option.flag==NULL，则返回值为option.val。如返回-1表示处理完毕。
extern int getopt_long_only (int argc, char *const argv[],
			     const char *optstring,
		         const struct option *longopts, int *longindex);
```

