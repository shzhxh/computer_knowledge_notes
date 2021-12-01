#### 概述

unistd是unix std的意思，由POSIX标准定义。包含了大量的对系统调用的封装。

#### 相关函数

```c
/*
 * 创建单路的管道。
 * 参数：向pipefd[1]的写入，在pipefd[0]能读出来。
 * 返回值：成功返回0，失败返回-1。
 */
int pipe(int pipefd[2]);

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

