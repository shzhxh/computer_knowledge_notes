#### 作用：
- 等待一个文件描述符上的事件。和系统调用select()的作用相近，它会等待一个文件描述符的集合是否准备好进行I/O。
#### 描述：
- 函数原型：`int poll(struct pollfd *fds, nfds_t nfds, int timeout)`
