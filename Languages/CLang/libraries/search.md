#### 概述

查找

#### 相关函数

队列管理

```c
/* 向一个队列中插入条目 */
void insque(void *elem, void *prev);

/* 从一个队列中删除元素 */
void remque(void *elem);
```

hsearch

```c
/* 哈希查找 */
int hcreate(size_t);
void hdestroy(void);
ENTRY *hsearch(ENTRY, ACTION);
```

tsearch

```c
void *tdelete(const void *__restrict, void **__restrict, int(*)(const void *, const void *));
void *tfind(const void *, void *const *, int(*)(const void *, const void *));
/* 二叉树查找 */
void *tsearch(const void *, void **, int (*)(const void *, const void *));
void twalk(const void *, void (*)(const void *, VISIT, int));
```

lsearch

```c
/* 对数组的线性查找 */
void *lsearch(const void *, void *, size_t *, size_t, int (*)(const void *, const void *));
void *lfind(const void *, const void *, size_t *, size_t, int (*)(const void *, const void *));
```

