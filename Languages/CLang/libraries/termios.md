终端属性的获取与设置、行控制、波特率的获取与设置

参考`man termios`

数据结构：

```c
#define NCCS 32
struct termios
  {
    tcflag_t c_iflag;		/* input mode flags */
    tcflag_t c_oflag;		/* output mode flags */
    tcflag_t c_cflag;		/* control mode flags */
    tcflag_t c_lflag;		/* local mode flags */
    cc_t c_line;			/* line discipline */
    cc_t c_cc[NCCS];		/* control characters */
    speed_t c_ispeed;		/* input speed */
    speed_t c_ospeed;		/* output speed */
#define _HAVE_STRUCT_TERMIOS_C_ISPEED 1
#define _HAVE_STRUCT_TERMIOS_C_OSPEED 1
  };
```

函数：

```c
/* 刷新FD上挂起的数据
 * 参数：__queue_selector
 *	 TCIFLUSH:0,
 *	 TCOFLUSH:1,
 *	 TCIOFLUSH:2,
 */
extern int tcflush (int __fd, int __queue_selector) __THROW;
/*
 * 暂停或重启FD上的传输
 * 参数：__action
 *	 TCOOFF:0,
 *   TCOON:1,
 *   TCIOFF:2,
 *   TCION:3,
 */
extern int tcflow (int __fd, int __action) __THROW;
```

