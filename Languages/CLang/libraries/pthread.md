#### 概述

线程管理

#### 相关函数

##### 管理tls

(Thread Local Storage)

```c
/* 创建一个新的线程 */
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);

/* 加入一个终止的线程。即等待由thread定义的线程结束。
 * 如果retval非NULL，则目标线程把退出状态复制给retval。
 * 返回值：０成功，失败则返回错误号。
 */
int pthread_join(pthread_t thread, void **retval);
```

##### 管理cancel

```c
int pthread_setcancelstate(int, int *);
int pthread_setcanceltype(int, int *);
void pthread_testcancel(void);
/* 向线程发送一个取消请求 */
int pthread_cancel(pthread_t);
```



##### 管理互斥锁

```c
int pthread_mutex_init(pthread_mutex_t *__restrict, const pthread_mutexattr_t *__restrict);
int pthread_mutex_lock(pthread_mutex_t *);
int pthread_mutex_unlock(pthread_mutex_t *);
int pthread_mutex_trylock(pthread_mutex_t *);
int pthread_mutex_timedlock(pthread_mutex_t *__restrict, const struct timespec *__restrict);
int pthread_mutex_destroy(pthread_mutex_t *);
int pthread_mutex_consistent(pthread_mutex_t *);
```

##### 互斥锁的属性

```c
int pthread_mutexattr_destroy(pthread_mutexattr_t *);
int pthread_mutexattr_getprioceiling(const pthread_mutexattr_t *__restrict, int *__restrict);
int pthread_mutexattr_getprotocol(const pthread_mutexattr_t *__restrict, int *__restrict);
int pthread_mutexattr_getpshared(const pthread_mutexattr_t *__restrict, int *__restrict);
int pthread_mutexattr_getrobust(const pthread_mutexattr_t *__restrict, int *__restrict);
int pthread_mutexattr_gettype(const pthread_mutexattr_t *__restrict, int *__restrict);
int pthread_mutexattr_init(pthread_mutexattr_t *);
int pthread_mutexattr_setprioceiling(pthread_mutexattr_t *, int);
int pthread_mutexattr_setprotocol(pthread_mutexattr_t *, int);
int pthread_mutexattr_setpshared(pthread_mutexattr_t *, int);
int pthread_mutexattr_setrobust(pthread_mutexattr_t *, int);
int pthread_mutexattr_settype(pthread_mutexattr_t *, int);
```

##### 管理条件变量

```c
int pthread_cond_init(pthread_cond_t *__restrict, const pthread_condattr_t *__restrict);
int pthread_cond_destroy(pthread_cond_t *);
int pthread_cond_wait(pthread_cond_t *__restrict, pthread_mutex_t *__restrict);
int pthread_cond_timedwait(pthread_cond_t *__restrict, pthread_mutex_t *__restrict, const struct timespec *__restrict);
int pthread_cond_broadcast(pthread_cond_t *);
int pthread_cond_signal(pthread_cond_t *);
```

##### 管理屏障

```c
// 屏障：就是高级的实时线程

/* 作用：销毁屏障及它使用的所有资源
 * barrier：要销毁的屏障
 */
int pthread_barrier_destroy(pthread_barrier_t *barrier);
/* 作用：为屏障barrier分配资源，并依据属性attr初始化屏障
 * barrier：要分配资源的屏障
 * attr：属性。如为NULL，则使用默认的屏障属性。
 * count：在成功返回前调用pthread_barrier_wait()的线程数。
 */
int pthread_barrier_init(pthread_barrier_t *restrict barrier,
const pthread_barrierattr_t *restrict attr, unsigned count); 
/* 
 * 作用：各个设置了屏障的线程通过调用此函数实现同步。
 */
int pthread_barrier_wait(pthread_barrier_t *barrier);
```

##### 管理tsd

(Thread Specific Data)

```c
/* 创建一个tsd */
int pthread_key_create(pthread_key_t *, void (*)(void *));
/* 注销一个tsd */
int pthread_key_delete(pthread_key_t);
/* 获取tsd */
void *pthread_getspecific(pthread_key_t);
/* 存储tsd */
int pthread_setspecific(pthread_key_t, const void *);
```

#### 
