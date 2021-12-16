#### 概述

消息队列。关于POSIX消息队列详见`man 7 mq_overview`。

#### 数据结构

```c
struct mq_attr {
	long mq_flags;       /* Flags (ignored for mq_open()) */
	long mq_maxmsg;      /* Max. # of messages on queue */
	long mq_msgsize;     /* Max. message size (bytes) */
	long mq_curmsgs;     /* # of messages currently in queue
                                       (ignored for mq_open()) */
};
```



#### 函数

```c
/* 打开或创建一个POSIX消息队列
 * name : 用于标识消息队列
 * oflag : 描述进行什么操作，详见<fcntl.h>。
 	O_RDWR - 打开队列后即可以接收，也可以发送消息
 	O_CREAT - 消息队列如不存在则创建之。
 	O_NONBLOCK - 以非阻塞模式打开队列。
 * 返回值：成功，则返回消息队列描述符；失败，返回(mqd_t) -1。
 */
mqd_t mq_open(const char *name, int oflag);
/* 打开或创建一个POSIX消息队列。如在oflag里定义了O_CREAT，则必须附加后面的两个参数。
 * mode : 定义了新队列的权限，像open()那样。
 * attr : 关于消息队列里消息的信息。
 * 返回值：成功，则返回消息队列描述符；失败，返回(mqd_t) -1。
 */
mqd_t mq_open(const char *name, int oflag, mode_t mode,
                     struct mq_attr *attr);

/* 关闭消息队列 */
int mq_close(mqd_t mqdes);

/* 删除消息队列 */
int mq_unlink(const char *name);

/* 向消息队列发消息
 * mqdes : 消息队列的描述符
 * msg_ptr : 代表了要向消息队列发的消息
 * msg_len : 要发的消息的长度，可以是0。注：必须小于每条消息允许的最大长度mq_msgsize。
 * msg_prio : 要发的消息的优先级，要求是非负整数。
 * 返回值 : 0成功；-1失败。
*/
int mq_send(mqd_t mqdes, const char *msg_ptr,
                     size_t msg_len, unsigned int msg_prio);

/* 从消息队列收消息 */
ssize_t mq_receive(mqd_t mqdes, char *msg_ptr,
                          size_t msg_len, unsigned int *msg_prio);

/* 从消息队列收消息
 * abs_timeout : 接收消息时允许等待的时间。如果消息队列为空且打开消息队列的时候没有指定O_NONBLOCK标志，则此参数有效。
 */
ssize_t mq_timedreceive(mqd_t mqdes, char *msg_ptr,
                          size_t msg_len, unsigned int *msg_prio,
                          const struct timespec *abs_timeout);
```

