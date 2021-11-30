#### 概述

日期与时间处理

```c
int clock_getres(clockid_t clk_id, struct timespec *res);

/* 获取时钟clk_id的时间 */
int clock_gettime(clockid_t clk_id, struct timespec *tp);

/* 设置时钟clk_id的时间 */
int clock_settime(clockid_t clk_id, const struct timespec *tp);

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
/* 高精度的睡眠 */
int nanosleep(const struct timespec *req, struct timespec *rem);
```

