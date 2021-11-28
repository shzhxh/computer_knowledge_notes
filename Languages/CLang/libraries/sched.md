#### 概述

管理调度器

#### 相关函数

##### 管理调度器

```c
/* 为进程设置调度参数  */
extern int sched_setparam (__pid_t __pid, const struct sched_param *__param)
     __THROW;

/* 获取进程的调度参数  */
extern int sched_getparam (__pid_t __pid, struct sched_param *__param) __THROW;

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

/* 让出处理器  */
extern int sched_yield (void) __THROW;

/* 获取调度器的最高优先级的值  */
extern int sched_get_priority_max (int __algorithm) __THROW;

/* 获取调度器的最小优先级的值  */
extern int sched_get_priority_min (int __algorithm) __THROW;

/* 获取进程__pid的SCHED_RR间隔  */
extern int sched_rr_get_interval (__pid_t __pid, struct timespec *__t) __THROW;
```

##### 管理线程的亲和性

```c
/* 设置线程对CPU的亲和性 */
int sched_setaffinity(pid_t pid, size_t cpusetsize,
	const cpu_set_t *mask);

/* 获取线程对CPU的亲和性
 * pid：待获取线程的pid。如为0，则获取的是当前线程的亲和性。
 * cupsetsize：即mask所指向的数据的大小
 * mask：代表了cpu的集合
 */
int sched_getaffinity(pid_t pid, size_t cpusetsize,
	cpu_set_t *mask);
```



##### 操作CPU集合的宏

```c
/*
  cpu_set_t的解释：
  它代表了一个CPU的集合，它实现为位的掩码。
  它应该被认为是不透明的，所有操作都应该通过宏来实现。
  
  typedef struct{
  unsigned long __bits[__CPU_SETSIZE / __NCPUBITS];
} cpu_set_t;

 */

/* 清空集合set，即让它不包含任何CPU */
void CPU_ZERO(cpu_set_t *set);

/* 把CPU"cpu"加到集合set里 */
void CPU_SET(int cpu, cpu_set_t *set);
/*  */
void CPU_CLR(int cpu, cpu_set_t *set);
/* 判断CPU"cpu"在不在集合set里
 * 返回值：非0则在集合里，0则不在集合里。
 */
int  CPU_ISSET(int cpu, cpu_set_t *set);

/*  */
int  CPU_COUNT(cpu_set_t *set);

/*  */
void CPU_AND(cpu_set_t *destset,
	cpu_set_t *srcset1, cpu_set_t *srcset2);
/*  */
void CPU_OR(cpu_set_t *destset,
	cpu_set_t *srcset1, cpu_set_t *srcset2);
/*  */
void CPU_XOR(cpu_set_t *destset,
	cpu_set_t *srcset1, cpu_set_t *srcset2);

/*  */
int  CPU_EQUAL(cpu_set_t *set1, cpu_set_t *set2);

/*  */
cpu_set_t *CPU_ALLOC(int num_cpus);
/*  */
void CPU_FREE(cpu_set_t *set);
/*  */
size_t CPU_ALLOC_SIZE(int num_cpus);

/*  */
void CPU_ZERO_S(size_t setsize, cpu_set_t *set);

/*  */
void CPU_SET_S(int cpu, size_t setsize, cpu_set_t *set);
/*  */
void CPU_CLR_S(int cpu, size_t setsize, cpu_set_t *set);
/*  */
int  CPU_ISSET_S(int cpu, size_t setsize, cpu_set_t *set);

/*  */
int  CPU_COUNT_S(size_t setsize, cpu_set_t *set);

/*  */
void CPU_AND_S(size_t setsize, cpu_set_t *destset,
	cpu_set_t *srcset1, cpu_set_t *srcset2);
/*  */
void CPU_OR_S(size_t setsize, cpu_set_t *destset,
	cpu_set_t *srcset1, cpu_set_t *srcset2);
/*  */
void CPU_XOR_S(size_t setsize, cpu_set_t *destset,
	cpu_set_t *srcset1, cpu_set_t *srcset2);

/*  */
int  CPU_EQUAL_S(size_t setsize, cpu_set_t *set1, cpu_set_t *set2);

```

