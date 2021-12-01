#### 概述

文件操控

#### 相关函数

```c
/* 相关数据结构 */
struct flock {
	...
	short l_type;    /* Type of lock: F_RDLCK,
                        F_WRLCK, F_UNLCK */
	short l_whence;  /* How to interpret l_start:
                       SEEK_SET, SEEK_CUR, SEEK_END */
	off_t l_start;   /* Starting offset for lock */
	off_t l_len;     /* Number of bytes to lock */
	pid_t l_pid;     /* PID of process blocking our lock
                        (set by F_GETLK and F_OFD_GETLK) */
	...
};

/* 参数cmd可能的值 */
// 1 复制文件描述符
// 2 文件描述符的标志
// 3 文件状态的标志
// 4 advisory记录锁定
//	F_SETLK (struct flock *) : 请求或释放锁。
//		当参数里的l_type为F_RDLCK或F_WRLCK,则请求锁。
//		当参数里的l_type为F_UNLCK则释放锁。
// 5 打开的文件描述锁定
// 6 强制锁定
// 7 丢失锁
// 8 管理信号
// 9 出租
// 10 文件与目录改变通知
// 11 改变管道
// 12 文件密封
// 13 文件读写提示


/* fcntl ：依据命令cmd操作文件fd
 * 返回值：如成功，取决于cmd；如失败，返回-1。
 *		F_DUPFD - 
 *		F_GETFD -
 *		F_GETFL -
 *		F_GETLEASE - 
 *		F_GETTOWN - 
 *		F_GETSIG - 
 *		F_GETPIPE_SZ, F_SETPIPE_SZ - 管道容量
 *		F_GET_SEALS - 
 *		其它 - 0
 */
int fcntl(int fd, int cmd, ... /* arg */ ); 
```

