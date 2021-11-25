```c
/* 设置时间 */
int gettimeofday(struct timeval *tv, struct timezone *tz);
/* 获取时间 */
int settimeofday(const struct timeval *tv, const struct timezone *tz);
```