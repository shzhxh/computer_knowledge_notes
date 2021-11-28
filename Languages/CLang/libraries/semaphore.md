#### 概述

管理信号量

#### 相关函数

```c
int    sem_close(sem_t *);

/* 销毁一个未命名的信号量 */
int    sem_destroy(sem_t *);

/* 获取信号量的值 */
int    sem_getvalue(sem_t *__restrict, int *__restrict);

/* 初始化一个未命名的信号量 */
int    sem_init(sem_t *, int, unsigned);

sem_t *sem_open(const char *, int, ...);

/* 给一个信号量解锁 */
int    sem_post(sem_t *);

int    sem_unlink(const char *);

/* 给信号量上锁 */
int    sem_wait(sem_t *);
int    sem_timedwait(sem_t *__restrict, const struct timespec *__restrict);
int    sem_trywait(sem_t *);
```

