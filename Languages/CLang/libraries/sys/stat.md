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
	dev_t     st_dev;         /* ID of device containing file */
	ino_t     st_ino;         /* Inode number */
	mode_t    st_mode;        /* File type and mode */
	nlink_t   st_nlink;       /* Number of hard links */
	uid_t     st_uid;         /* User ID of owner */
	gid_t     st_gid;         /* Group ID of owner */
	dev_t     st_rdev;        /* Device ID (if special file) */
	off_t     st_size;        /* Total size, in bytes */
	blksize_t st_blksize;     /* Block size for filesystem I/O */
	blkcnt_t  st_blocks;      /* Number of 512B blocks allocated */

    /* 2.6内核之后提高到ns精度，增加了如下的域 */
	struct timespec st_atim;  /* Time of last access */
	struct timespec st_mtim;  /* Time of last modification */
	struct timespec st_ctim;  /* Time of last status change */

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

