#### 概述

定义System V IPC相关的函数。

#### 相关函数

```c
/* 使用文件名和至少8位proj_id生成System V IPC关键字
 * 返回值：成功则返回关键字的值，失败则返回-1。
 */
key_t ftok(const char *pathname, int proj_id);
```

