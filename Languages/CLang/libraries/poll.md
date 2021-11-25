#### 
```c
/*
 * 等待一个文件描述符上的事件。和系统调用select()的作用相近，它会等待文件描述符的集合里的其中一个文件描述符，直到它准备好进行I/O。
 * fds：结构体pollfd的指针，包含了文件描述符的集合
 * nfds：指针fds包含的元素个数，即结构体pollfd的个数
 * timeout：等待的文件描述符的时间，超过此时间将不再等待。单位：毫秒。
 */
int poll(struct pollfd *fds, nfds_t nfds, int timeout)
    
struct pollfd{
    int fd;			// 文件描述符
    short events;	// 请求的事件，是一个输入参数
    short revents;	//　返回的事件，是一个输出参数
}

// events和revents可能的值：
//   POLLIN - 存在可读的数据
```

