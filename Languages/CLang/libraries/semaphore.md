#### 概述

glibc提供了两种类型的信号量：POSIX信号量和SYS V信号量。在`semaphore.h`里提供的是POSIX信号量，即本文件描述的是POSIX信号量。POSIX信号量又分两种：无名信号量和有名信号量。

##### 无名信号量

无名信号量的数据类型`sem_t`定义在bits/semaphore.h。

```c
#if __WORDSIZE == 64
# define __SIZEOF_SEM_T 32
#else
# define __SIZEOF_SEM_T 16
#endif

typedef union
{
  char __size[__SIZEOF_SEM_T];
  long int __align;
} sem_t;
```

##### 有名信号量

有名信号量的值保存在文件中。

#### 相关函数

##### 有名信号量

```c
/* 销毁一个无名信号量 */
int    sem_destroy(sem_t *);

/* 获取信号量的值 */
int    sem_getvalue(sem_t *__restrict, int *__restrict);

/* 初始化一个无名信号量 */
int    sem_init(sem_t *, int, unsigned);

/* 给一个信号量解锁。即V操作，释放资源。
 * 注：有名信号量和无名信号量共用此函数。
 */
int    sem_post(sem_t *);

/* 给信号量上锁。即P操作，申请资源。
 * 注：有名信号量和无名信号量共用此函数。
 */
int    sem_wait(sem_t *);
int    sem_timedwait(sem_t *__restrict, const struct timespec *__restrict);
int    sem_trywait(sem_t *);
```

##### 无名信号量

```c
/* 打开一个有名信号量。 */
sem_t *sem_open(const char *, int, ...);
/* 关闭一个有名信号量。 */
int    sem_close(sem_t *);
/* 删除一个有名信号量。 */
int    sem_unlink(const char *);
```



#### 参考文档

- `man 7 sem_overview`

- [linengier - Linux系统编程之信号量](https://www.cnblogs.com/linengier/p/9399880.html#:~:text=%E5%9C%A8%E5%AD%A6%E4%B9%A0%E4%BF%A1%E5%8F%B7%E9%87%8F%E4%B9%8B%E5%89%8D%EF%BC%8C%E6%88%91%E4%BB%AC%E5%BF%85%E9%A1%BB%E5%85%88%E7%9F%A5%E9%81%93%E2%80%94%E2%80%94Linux%E6%8F%90%E4%BE%9B%E4%B8%A4%E7%A7%8D%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%9A%20%EF%BC%881%EF%BC%89,%E5%86%85%E6%A0%B8%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%8C%E7%94%B1%E5%86%85%E6%A0%B8%E6%8E%A7%E5%88%B6%E8%B7%AF%E5%BE%84%E4%BD%BF%E7%94%A8%20%EF%BC%882%EF%BC%89%20%E7%94%A8%E6%88%B7%E6%80%81%E8%BF%9B%E7%A8%8B%E4%BD%BF%E7%94%A8%E7%9A%84%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%8C%E8%BF%99%E7%A7%8D%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%8F%88%E5%88%86%E4%B8%BAPOSIX%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%92%8CSYSTEMV%E4%BF%A1%E5%8F%B7%E9%87%8F%E3%80%82)
