#### 概述

设置文件的状态

#### 相关函数

##### 时间戳精度

```c
/* 把文件的时间戳提高到纳秒的精度 */
int utimensat(int dirfd, const char *pathname, const struct timespec times[2], int flags);
int futimens(int fd, const struct timespec times[2]);
```

##### 文件状态

```c
/* stat代表了文件的状态，其对应的数据结构为 */
struct stat {
	dev_t     st_dev;         /* 设备ID */
	ino_t     st_ino;         /* Inode号 */
	mode_t    st_mode;        /* 文件的类型与模式 */
	nlink_t   st_nlink;       /* 硬链接的数量 */
	uid_t     st_uid;         /* 属主的用户ID */
	gid_t     st_gid;         /* 属主的组ID */
	dev_t     st_rdev;        /* 设备的主、次设备号 */
	off_t     st_size;        /* 文件大小，以字节为单位 */
	blksize_t st_blksize;     /* Block size for filesystem I/O */
	blkcnt_t  st_blocks;      /* Number of 512B blocks allocated */

    /* 2.6内核之后提高到ns精度，增加了如下的域 */
	struct timespec st_atim;  /* 最后访问时间 */
	struct timespec st_mtim;  /* 修改时间 */
	struct timespec st_ctim;  /* 状态改变时间 */

	#define st_atime st_atim.tv_sec      /* Backward compatibility */
	#define st_mtime st_mtim.tv_sec
	#define st_ctime st_ctim.tv_sec
};


/* 获取文件名为pathname的文件的状态。如pathname是符号链接，则获取它指向的文件的信息。
 * 返回值：0成功；-1失败。
 */
int stat(const char *pathname, struct stat *statbuf);
/* 获取文件描述符为fd的文件的状态。 */
int fstat(int fd, struct stat *statbuf);
/* 获取文件名为pathname的文件的状态。如pathname是符号链接，则获取链接的信息。 */
int lstat(const char *pathname, struct stat *statbuf);
int fstatat(int dirfd, const char *pathname, struct stat *statbuf,
			int flags);
```

