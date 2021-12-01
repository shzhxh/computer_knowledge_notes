#### 概述

内存映射管理

#### 映射和解除映射

```c
/*
 * 作用：把文件或设备映射到内存
 * addr：将映射到的内存地址。如为NULL，则内核会选择创建映射的地址。
 * length：映射区域的大小
 * prot：映射区域的保护方式。PROT_EXEC可执行，PROT_READ可读，PROT_WRITE可写，PROT_NONE不可访问。
 * flags：映射区域的特性。MAP_SHARED共享此映射，MAP_ANONYMOUS不支持任何文件，此时忽略参数fd且offset应为0。
 * fd：文件描述符
 * offset：文件偏移量
 * 返回值：成功则返回映射到的地址，失败则返回(void *)-1。
 */
void *mmap(void *addr, size_t length, int prot, int flags,
                  int fd, off_t offset);

/*
 * 作用：取消文件或设备到内存的映射
 */
int munmap(void *addr, size_t length);
```

#### 锁定和解锁内存

```c
/* 
 * 作用：锁定调用进程的内存，防止换页到交换区。
 */
int mlock(const void *addr, size_t len);
int mlock2(const void *addr, size_t len, int flags);
/*
 * 作用：锁定调用进程的全部内存，防止换页到交换区。
 * flag：MCL_CURRENT锁定当前映射的所有内存页，MCL_FUTURE以后被映射的内存页也会被锁定。
 * 返回值：0成功，-1失败。
 */
int mlockall(int flags);

int munlock(const void *addr, size_t len);
int munlockall(void);
```

#### POSIX共享内存对象

```c
/* 创建或打开一个POSIX共享内存对象
 * name：内存对象的名字
 * oflag：必须包含O_RDONLY和O_RDWR其中之一，其余可选择包含：
 *		O_CREAT,如不存在则创建
 *		O_EXCL, 如指定了O_CREAT且存在，则返回错误。
 *		O_TRUNC，如存在则截断为0字节
 * mode：详见sys/stat.h
 *		S_IRUSER, 所有者可读
 *		S_IWUSER, 所有者可写
 *		S_IXUSER, 所有者可执行
 * 返回值：成功则返回一个非负的文件描述符；失败返回-1。
 */
int shm_open(const char *name, int oflag, mode_t mode);

/* 删除一个共享内存对象
 * 返回值：0成功，-1失败。
 */
int shm_unlink(const char *name);
```

