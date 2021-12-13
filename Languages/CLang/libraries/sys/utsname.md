#### 概述

系统识别。uts的意思是UNIX Timesharing System。

背景知识：Linux有６种命名空间。UTS, User, Mount, IPC, PID, Net。

相关数据结构

```c
/* in sys/utsname.h */
struct utsname {
	char sysname[];    /* Operating system name (e.g., "Linux") */
	char nodename[];   /* Name within "some implementation-defined
                                     network" */
	char release[];    /* Operating system release (e.g., "2.6.28") */
	char version[];    /* Operating system version */
	char machine[];    /* Hardware identifier */
#ifdef _GNU_SOURCE
	char domainname[]; /* NIS or YP domain name */
#endif
};

```

相关函数

```c
/* 获取当前内核的信息，将其保存在buf里。
 * 返回值：0,成功；-1，失败。
*/
int uname(struct utsname *buf);
```

