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

