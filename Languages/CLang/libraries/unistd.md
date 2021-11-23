```c
int pipe(int pipefd[2]);	// 创建单路的管道。向pipefd[1]的写入，在pipefd[0]能读出来。成功返回0，失败返回-1。
```

