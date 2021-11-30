#### 概述

SYS V信号量。

SYS V信号量是System V进程间通信的一部分。System V进程间通信的方式包括：消息队列、信号量和共享内存。

`semid_ds`描述了一组信号量，它定义在`bits/sem.h`：

```c
/* Data structure describing a set of semaphores.  */
struct semid_ds{
  struct ipc_perm sem_perm;             /* operation permission struct */
  __SEM_PAD_TIME (sem_otime, 1);        /* last semop() time */
  __SEM_PAD_TIME (sem_ctime, 2);        /* last time changed by semctl() */
  __syscall_ulong_t sem_nsems;          /* number of semaphores in set */
  __syscall_ulong_t __glibc_reserved3;
  __syscall_ulong_t __glibc_reserved4;
};
```



#### 相关函数

```c
/* System V信号量集合里的每个信号量都有如下相关变量(由内核维护，仅内核可见) */
unsigned short  semval;   /* semaphore value */
unsigned short  semzcnt;  /* # waiting for zero */
unsigned short  semncnt;  /* # waiting for increase */
pid_t           sempid;   /* PID of process that last */

/* sembuf定义了对单个信号量的操作 */
struct sembuf{
  unsigned short int sem_num;   /* 要操作的信号量在信号量集合中的编号 */
  short int sem_op;             /* 信号量操作 */
  short int sem_flg;            /* 操作标志符 */
    	// 可接受的sem_flg:IPC_NOWAIT and SEM_UNDO
    	// SEM_UNDO - 由进程自动释放信号量
    	// IPC_NOWAIT - 不阻塞
};

/* 功能：System V信号量集合的控制操作。
 * semid：要控制的信号量集合。
 * semnum：信号量在信号量集合中的序号。(从0开始编号。)
 * cmd：对信号量集合的控制操作。
 */
int semctl(int semid, int semnum, int cmd, ...);

/* 功能：获取System V信号量集合的描述符。semop()和semctl()会使用它。
 * key：返回的信号量集合描述符是与本参数相关联的，一个key只对应一个固定的描述符。
 *		IPC_PRIVATE：
 * nsems：信号量集合里的元素数。
 * semflg：
 *		IPC_CREATE：
 *		IPC_EXCL：
 * 返回值：成功则返回信号量集合的描述符(非负整数)；失败则返回-1。
 */
int semget(key_t key, int nsems, int semflg);

/* 功能：System V信号量操作。即P/V操作。
 * semid：信号量集合的描述符。执行semctl()函数可得。
 * sops：是一个数组指针，这个数组的元素是结构体sembuf。
 * nsops：数组spos里的元素个数。
 */
int semop(int semid, struct sembuf *sops, size_t nsops);
```

#### 参考文档

- [linengier - Linux系统编程之信号量](https://www.cnblogs.com/linengier/p/9399880.html#:~:text=%E5%9C%A8%E5%AD%A6%E4%B9%A0%E4%BF%A1%E5%8F%B7%E9%87%8F%E4%B9%8B%E5%89%8D%EF%BC%8C%E6%88%91%E4%BB%AC%E5%BF%85%E9%A1%BB%E5%85%88%E7%9F%A5%E9%81%93%E2%80%94%E2%80%94Linux%E6%8F%90%E4%BE%9B%E4%B8%A4%E7%A7%8D%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%9A%20%EF%BC%881%EF%BC%89,%E5%86%85%E6%A0%B8%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%8C%E7%94%B1%E5%86%85%E6%A0%B8%E6%8E%A7%E5%88%B6%E8%B7%AF%E5%BE%84%E4%BD%BF%E7%94%A8%20%EF%BC%882%EF%BC%89%20%E7%94%A8%E6%88%B7%E6%80%81%E8%BF%9B%E7%A8%8B%E4%BD%BF%E7%94%A8%E7%9A%84%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%8C%E8%BF%99%E7%A7%8D%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%8F%88%E5%88%86%E4%B8%BAPOSIX%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%92%8CSYSTEMV%E4%BF%A1%E5%8F%B7%E9%87%8F%E3%80%82)