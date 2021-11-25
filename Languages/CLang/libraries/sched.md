

```c
/* 为进程pid设置调度算法policy和(或)参数param
 * 如pid为0，则设置的是调用线程的调度算法和参数
　* 参数param依赖于policy
　* 返回值：０成功，-1失败。
　*/
int sched_setscheduler(pid_t pid, int policy,
                       const struct sched_param *param);
// sched_param的结构
// 如policy为SCHED_OTHER, SCHED_BATCH, SCHED_IDLE,则sched_priority必须为０
// 如policy为SCHED_FIFO，SCHED_RR，则sched_priority的范围为1~99，99优先级最高
struct sched_param {
               int sched_priority;
};

/* 返回线程pid的调度算法，是一个非负的整数。如失败，则返回-1。
 * 如pid为0，则获取的是调用线程的调度算法。
 */
int sched_getscheduler(pid_t pid);
```

