#### 概述

设置时间

#### 相关结构体

```c
struct timeval {
	time_t      tv_sec;     /* seconds */
	suseconds_t tv_usec;    /* microseconds */
};

struct timezone {
	int tz_minuteswest;     /* minutes west of Greenwich */
	int tz_dsttime;         /* type of DST correction */
};
```



#### 相关函数

##### 时间管理

```c
/* 获取时间和时区 */
int gettimeofday(struct timeval *tv, struct timezone *tz);
/* 设置时间和时区 */
int settimeofday(const struct timeval *tv, const struct timezone *tz);
```

##### 间隔计时器管理

间隔计时器：初次超时时间是未来某个点，在此之后是规律的间隔超时。当计时器超时的时候，会向调用进程发信号，计时器会重置时间间隔(如果这个间隔不为0的话)。

```c
/* 参数which可能的值：
 *	ITIMER_REAL - 计时器以实际的时间倒数。超时的时候发的信号是SIGALRM。
 *	ITIMER_VIRTUAL - 计时器根据用户模式下的进程消耗的CPU时间倒数。超时的时候发的信号是SIGVTALRM。
 *	ITIMER_PROF - 计时器依据进程消耗的CPU总时间倒数，即包括用户模式也包括系统模式。超时的时候发的信号是SIGPROF。
 */

/* 获取间隔计时器的值 */
int getitimer(int which, struct itimerval *curr_value);
/* 开启或停止间隔计时器
 * which：要控制的计时器
 * new_value：计时器要设置的值。如果它的it_value域非0，则计时器启动并代表了初次超时的时间。如果它的it_value域为0，则停止计时器。如果它的it_interval域非0，则代表了超时的间隔。如果它的it_interval域为0，则仅超时一次。
 * old_value：非NULL则保存之前的值
 * 返回值：0成功；-1失败。
 */
int setitimer(int which, const struct itimerval *new_value,
                     struct itimerval *old_value);
```

