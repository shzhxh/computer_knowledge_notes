#### 概述

处理信号

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

#### 设置信号

```c
/* 将信号集set初始化为空 */
int sigemptyset(sigset_t *set);
/* 将信号集set初始化为包含所有信号 */
int sigfillset(sigset_t *set);
/* 把信号signum添加到信号集set里 */
int sigaddset(sigset_t *set, int signum);
/* 把信号signum从信号集set中删除 */
int sigdelset(sigset_t *set, int signum);
/* 判断信号signum是否在信号集set里
 * 返回值：1包含，0不包含，-1信号无效
 */
int sigismember(const sigset_t *set, int signum);

```

#### 检测和改变屏蔽的信号

```c
/*
 * 作用：取决于参数how。
 * how：SIG_BLOCK把参数set中的信号添加到信号屏蔽字中，
 *      SIG_SETMASK把信号屏蔽字设置为参数set中的信号，
 *		SIG_UNMASK从信号屏蔽字中删除参数set中的信号
 * set：信号的集合。
 * oldset：保存之前的信号屏蔽字。
 */
int sigpromask(int how, const sigset_t *set, sigset_t *oset);
/* 作用：和sigprocmask一样，不同之处在于它适用于多线程程序 */
int pthread_sigmask(int how, const sigset_t *set, sigset_t *oldset);
```



