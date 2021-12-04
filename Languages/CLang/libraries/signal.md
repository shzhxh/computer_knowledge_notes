#### 概述

处理信号。信号是发给进程或线程的异步通知。信号源产生信号，内核接收到信号，然后内核把信号发给对应的进程或线程。

使用`kill -l`查看信号值和信号名。使用`man 7 signal`查看信号的详细信息。

信号集：信号的集合，是一个64位的数据类型(因为一共有64种信号，一位代表一个信号)。1已注册，0未注册。

抵达(delivery)：当进程收到信号后所做的动作。抵达有三种：

- 忽略此信号。
- 捕获此信号，并执行一个特殊函数handler。
- 执行信号的默认行为。

未决(pending)：信号被阻塞。

信号屏蔽状态字(signal mask)：在PCB的结构体中，每位对应一个信号。1屏蔽，0不屏蔽。

信号未决状态字(mask of pending signals)：在PCB的结构体中，每位对应一个信号。1未决，0不抵达。

#### 信号

```c
#define	SIGHUP		1	// 挂起（当进程的控制进程死亡时向进程发送此信号）
#define	SIGINT		2	// 中断（当键盘中断发生时控制终端向进程发送此信号）
#define	SIGKILL		9	// 立即终止进程(此信号不能被捕获或忽略)
#define	SIGTERM		15	// 要求进程终止
```

#### 管理信号

```c
typedef void (*sighandler_t)(int);

/*
 * 作用：把信号signum设置给handler处理。
 * 库里定义了三个假的handler：
 *   SIG_ERR：错误返回。
 *   SIG_DFL：默认动作。即关于此信号的默认动作将会执行。
 *   SIG_IGN：忽略信号。
 */
sighandler_t signal(int signum, sighandler_t handler);
```

#### 操作信号集

```c
/* 将信号集set初始化为空，即将所有位都设置为0
 * 返回值：0成功，-1出错。
 */
int sigemptyset(sigset_t *set);

/* 将信号集set初始化为包含所有信号，即将所有位都设置为1
 * 返回值：0成功，-1出错。
 */
int sigfillset(sigset_t *set);

/* 把信号signum添加到信号集set里。即将信号集里对应的位设置为1。
 * 返回值：0成功，-1出错。
 */
int sigaddset(sigset_t *set, int signum);

/* 把信号signum从信号集set中删除。即将信号集里对应的位设置为1。
 * 返回值：0成功，-1出错。
 */
int sigdelset(sigset_t *set, int signum);

/* 判断信号signum是否在信号集set里
 * 返回值：1包含，0不包含，-1信号无效
 */
int sigismember(const sigset_t *set, int signum);
```

#### 检测或改变信号屏蔽字

```c
/*
 * 作用：获取或改变调用线程的信号屏蔽字。只能用于单进程单线程。
 * how：SIG_BLOCK把参数set中的信号添加到信号屏蔽字中，
 *      SIG_SETMASK把信号屏蔽字设置为参数set中的信号，
 *		SIG_UNMASK从信号屏蔽字中删除参数set中的信号
 * set：信号集。
 * oldset：如非NULL，则保存之前的信号屏蔽字。
 * 返回值：0成功,-1失败。
 */
int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
int rt_sigprocmask(int how, const kernel_sigset_t *set,
                  kernel_sigset_t *oldset, size_t sigsetsize);
/* 作用：和sigprocmask一样，不同之处在于它适用于多线程程序 */
int pthread_sigmask(int how, const sigset_t *set, sigset_t *oldset);
```



