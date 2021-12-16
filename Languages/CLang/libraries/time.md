#### 概述

日期与时间处理

#### 相关变量类型

```c
/* timespec定义了时间的值，精度为ns */
struct timespec {
	time_t tv_sec;        /* seconds */
	long   tv_nsec;       /* ns,必须在0~999,999,999之间 */
};

/* POSIX.1b structure for timer start values and intervals.  */
struct itimerspec  {
	struct timespec it_interval;	/* Timer interval */
	struct timespec it_value;		/* Initial expiration */
};
```



#### 相关函数

##### 时钟与时间函数

```c
/* glibc和Linux内核支持的clk_id：
 * CLOCK_REALTIME - 系统范围的时钟，用于测量真实时间。
 * CLOCK_REALTIME_COARSE - 也属于CLOCK_REALTIME，更快但精度低。
 * CLOCK_MONOTONIC - 记录单调时间的时钟，即系统启动到现在的秒数。
 * CLOCK_MONOTONIC_COARSE - 也属于CLOCK_MONOTONIC，更快但精度低。
 * CLOCK_MONOTONIC_RAW
 * CLOCK_BOOTTIME
 * CLOCK_PROCESS_CPUTIME_ID
 * CLOCK_THREAD_CPUTIME_ID
 */

/* 查询时钟clk_id的精度，将精度存储在res中。
 * 返回值：0成功；1失败。
 */
int clock_getres(clockid_t clk_id, struct timespec *res);

/* 获取时钟clk_id的时间 */
int clock_gettime(clockid_t clk_id, struct timespec *tp);

/* 设置时钟clk_id的时间 */
int clock_settime(clockid_t clk_id, const struct timespec *tp);
```

##### 计时器

```c
/* 创建一个POSIX per-process计时器，创建后的计时器处于停止(disarmed)的状态
 * clockid - 新计时器用此时钟来测量时间。
 *	CLOCK_REALTIME
 *	CLOCK_MONOTONIC
 *	CLOCK_PROCESS_CPUTIME_ID
 *	CLOCK_THREAD_CPUTIME_ID
 * sevp - 当计时器超时的时候如何通知调用者。(注：此结构体使用前必须初始化，比如用memeset函数)
 *	sevp.sigev_notify可能的取值：
 *	SIGEV_NONE - 计时器超时的时候不进行异步通知。
 *	SIGEV_SIGNAL - 计时器超时的时候向进程发信号。
 *	SIGEV_THREAD - 计时器超时的时候调用sigev_notify_function。
 *	SIGEV_THREAD_ID - 计时器超时的时候向线程发信号，线程id定义在sigev_notify_thread_id。
 * timerid - 用于保存新创建的计时器的id，必须是一个非NULL的指针。
 * 返回值：0成功；-1失败。
 */
int timer_create(clockid_t clockid, struct sigevent *sevp, timer_t *timerid);
/* 删除计时器timerid
 * 返回值：0成功；-1失败。
 */
int timer_delete(timer_t timerid);

/* 获取计时器的状态 */
int timer_gettime(timer_t timerid, struct itimerspec *value);
/* 启动(armed)、停止(disarmed)计时器。
 * timerid:要操作的计时器。
 * flags:为0时表示new_value->it_value是计时器到期的时间间隔；为TIMER_ABSTIME时表示new_value->it_value是计时器到期的绝对时间。
 * new_value:包含两个域
 *	it_value：启动或停止计时器的控制域。非0则启动计时器并代表了初次超时时间，为0则停止计时器。
 *	it_interval：用于设置计时器循环的时间间隔。非0则计时器过期后会重新加载it_interval(意味着会循环下去)，为0则仅过期一次。
 * old_value:非空则保存之前的值
 */
int timer_settime(timer_t timerid, int flags, const struct itimerspec *restrict new_value, struct itimerspec *restrict old_value);

/* 获取POSIX per-process计时器的溢出计数 */
int timer_getoverrun(timer_t timerid); 
```



##### 未分类

```c
/* 格式化日期与时间 */
size_t strftime(char *s, size_t max, const char *format, const struct tm *tm);

clock_t clock (void);
time_t time (time_t *);
double difftime (time_t, time_t);
time_t mktime (struct tm *);
/* 把日期转换为1970-01-01以来的秒数 */
struct tm *gmtime (const time_t *);
struct tm *localtime (const time_t *);
char *asctime (const struct tm *);
char *ctime (const time_t *);
int timespec_get(struct timespec *, int);
/* 让调用线程进行高精度(纳秒)的睡眠。(注：调用线程被唤醒可能有一定的延迟，因为要等待CPU空闲。)
 * req：表示要求睡眠的时间
 * rem：有可能线程没有睡到req要求的时间。如rem非NULL，则用来记录剩余应该睡眠的时间。从而可以继续调用nanosleep从而达到指定的睡眠。
 * 返回值：如果睡足了要求的时间，返回0；否则，返回-1。如果是因为信号handler而中断了函数的调用，则errono设置为EINTR。
 */
int nanosleep(const struct timespec *req, struct timespec *rem);
/* 让调用线程进行高精度(纳秒)的睡眠。可以指定时钟。 */
int clock_nanosleep(clockid_t clock_id, int flags,
                           const struct timespec *request,
                           struct timespec *remain);
```

