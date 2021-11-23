处理异常

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

