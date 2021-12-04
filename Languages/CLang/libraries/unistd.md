#### 概述

unistd是unix std的意思，由POSIX标准定义。包含了大量的对系统调用的封装。

#### 相关函数

##### 创建管道

```c
/*
 * 创建单路的管道。
 * 参数：向pipefd[1]的写入，在pipefd[0]能读出来。
 * 返回值：成功返回0，失败返回-1。
 */
int pipe(int pipefd[2]);

int pipe2(int pipefd[2], int flags);
```

##### 截断文件

```c
/* 将文件名为path的文件截断到长度为length字节，截断的原则是多退少补
 * 返回值：0成功，-1失败。
 */
int truncate(const char *path, off_t length);

/* 将文件描述符为fd的文件截断到长度为length字节，截断的原则是多退少补
 * 返回值：0成功，-1失败。
 */
int ftruncate(int fd, off_t length);
```

##### 运行文件

`exec()`家族的后端是`execve()`系统调用，它们都是用新进程来替代当前进程。它们只有在发生错误才返回，返回值是-1。

- p - 表示可以在环境变量PATH里找可执行文件
- e - 表示可以通过参数指定环境，即`envp[]`
- v - 表示在参数里提供了指针数组，即`argv[]`。v和l是互斥的。
- l - 表示是以`const char *`的形式给出的参数。l和v互斥的。

```c
 extern char **environ;

/* 要执行的文件必须以路径的形式给出，即path */
int execl(const char *path, const char *arg, ...
                       /* (char  *) NULL */);

/* 如不包含斜杠，则在环境变量PATH里找可执行文件；否则执行指定路径上的文件 */
int execlp(const char *file, const char *arg, ...
                       /* (char  *) NULL */);

/* 要执行的文件必须以路径的形式给出，即path
 * 可以通过envp指定环境
 */
int execle(const char *path, const char *arg, ...
                       /*, (char *) NULL, char * const envp[] */);

/* 要执行的文件必须以路径的形式给出，即path */
int execv(const char *path, char *const argv[]);

/* 如不包含斜杠，则在环境变量PATH里找可执行文件；否则执行指定路径上的文件 */
int execvp(const char *file, char *const argv[]);

/* 如不包含斜杠，则在环境变量PATH里找可执行文件；否则执行指定路径上的文件
 * 可以通过envp指定环境
 */
int execvpe(const char *file, char *const argv[],
                       char *const envp[]);
```

##### 其它

```c
/* 从文件描述符fd里读取count个字节到到缓冲区buf里。注意：read会使文件位置增长。
 * 返回值：成功则返回读取的字节数，返回0代表已到了文件的末尾。错误则返回-1。
 */
ssize_t read(int fd, void *buf, size_t count);
/*
 * 从缓冲区buf写入count个字符到文件描述符fd。
 * 返回值：成功则返回写入的字节数。错误则返回-1。
 */
ssize_t write(int fd, const void *buf, size_t count);
/*
 * 在运行的时候获取配置信息
 * name: _SC_NPROCESSORS_ONLN当前在线的处理器数量
 * 		_SC_NPROCESSORS_CONF已配置的处理器数量
 */
long sysconf(int name);

/* 创建子进程
 * 返回值：成功，如是父进程则返回子进程的pid，如是子进程则返回0。失败，父进程返回-1，子进程不会创建。
 */
pid_t fork(void);
```

