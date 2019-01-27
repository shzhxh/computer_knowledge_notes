### 文件系统的逻辑结构

#### 访问接口层

向用户提供标准访问接口，屏蔽内核的接口，在访问接口层看到的是文件。(open,close,read,write,readdir)

#### 抽象层

为内核其它部分提供统一接口，屏蔽具体的文件系统实现，在抽象层看到的是文件。(file,dir,inode)

#### 文件系统层

为抽象层提供统一接口，向下访问外设接口，是文件系统的具体实现，在文件系统层看到的是块。（SuperBlock,root-dir inode,freemap,data,inode)  

#### 外设接口层

为文件系统层提供统一接口，向下访问各种具体设备的驱动，在外设接口层看到的是设备。(device,vfs_dev_t,stdin,stdout)

### sfs数据结构

#### 1. sfs_fs结构体

  ```c
  /* 描述的是整个文件系统 */
  struct sfs_fs {
      struct sfs_super super;        /* on-disk superblock */
      struct device *dev;           // 块数据，块大小，块操作的结构体
      struct bitmap *freemap;       /* blocks in use are mared 0 */
      bool super_dirty;             /* true if super/freemap modified */
      void *sfs_buffer;            // sfs文件系统在内存中缓冲区的地址
      semaphore_t fs_sem;          /* semaphore for fs */
      semaphore_t io_sem;          /* semaphore for io */
      semaphore_t mutex_sem;       /* semaphore for link/unlink and rename */
      list_entry_t inode_list;     /* inode linked-list */
      list_entry_t *hash_list;     /* inode hash linked-list */
  };
  ```

#### 2. sfs_super结构体

   ```c
   /* 超级块，从全局角度描述文件系统 */
   struct sfs_super {
       uint32_t magic;                                 /* magic number, should be SFS_MAGIC */
       uint32_t blocks;                                /* # of blocks in fs */
       uint32_t unused_blocks;                         /* # of unused blocks in fs */
       char info[SFS_MAX_INFO_LEN + 1];                /* infomation for sfs  */
   };
   ```

   ​

#### 3. sfs_inode结构体

  ```c
  /* 内存中的结点，从单个文件的角度描述文件 */
  struct sfs_inode {
      struct sfs_disk_inode *din;                     /* on-disk inode */
      uint32_t ino;                                   /* inode number */
      bool dirty;                                     /* true if inode modified */
      int reclaim_count;                              /* kill inode if it hits zero */
      semaphore_t sem;                                /* semaphore for din */
      list_entry_t inode_link;                        /* entry for linked-list in sfs_fs */
      list_entry_t hash_link;                         /* entry for hash linked-list in sfs_fs */
  };
  ```

  * dirty: 如果inode被修改了，则将dirty置为true

#### 4. sfs_disk_inode结构体

  ```c
  /* 硬盘上的结点，代表了硬盘上一个实际存在的文件 */
  struct sfs_disk_inode {
      uint32_t size;                                  /* size of the file (in bytes) */
      uint16_t type;                                  /* one of SYS_TYPE_* above */
      uint16_t nlinks;                                /* # of hard links to this file */
      uint32_t blocks;                                /* # of blocks */
      uint32_t direct[SFS_NDIRECT];                   /* direct blocks */
      uint32_t indirect;                              /* indirect blocks */
  //    uint32_t db_indirect;                           /* double indirect blocks */
  //   unused
  };
  ```

  * blocks: 此结点占用块的数量
  * direct: 是inode中直接块的数组，数组大小为12，每个数组项都是块的编号。由此可见，文件在硬盘中是离散存储的。
  * indirect: 是间接块,间接块存放的都是其它块的索引号

#### 5. sfs_disk_entry结构体

```c
/* 目录项，从文件路径的角度描述文件系统 */
struct sfs_disk_entry {
    uint32_t ino;                                   /* inode number */
    char name[SFS_MAX_FNAME_LEN + 1];               /* file name */
};
```



### sfs函数
1. sfs_dirent_read_nolock  
  作用：从硬盘块中读文件入口到内存entry中  
  原理：依据文件系统sfs，内存结点sin，索引号slot获取硬盘块的编号ino，将ino块里第一条数据读入entry中
2. sfs_bmap_load_nolock  
  作用：依据索引index和结点sin，找到硬盘块上的编号*ino_sore
3. sfs_bmap_get_nolock  
  作用：依据索引index和结点sin,找到硬盘块的编号*ino_store
4. sfs_rbuf  
  作用：读取一个硬盘块blkno中偏移地址offset，长度len的内容，到内存buf中
5. sfs_wbuf  
  作用：将内存地址buf，长度len的内容，写到硬盘块blkno偏移地址offset处。
6. sfs_rwblock_nolock  
  作用：读或写一个硬盘块  
  * sfs: sfs->dev提供io函数  
  * buf: 内存中块的基址  
  * blkno: 硬盘块的编号，blkno * SFS_BLKSIZE 就是硬盘上的偏移地址  
  * write: 为1时将内存中的内容写到硬盘中的块上，为0时将硬盘里块的内容读到内存中
7. sfs_rblock  
  作用：从硬盘块编号blkno开始，读取nblks个块的内容到内存buf中
8. sfs_wblock  
  作用：将内存buf开始，nblks个块的内容写到从块编号blkno起的硬盘上

#### sfs_io_nolock函数
* 作用：读或写一个文件的内容  
* 原理：一个文件要读写的内容可以看做是由3种类型的块组成的，第1个块，最后1个块和中间的块。文件在第1个块和最后1个块要读写的内容极有可能只是整个块的一部分。  
* sfs: 文件系统  
  sin：内存中的结点  
  buf：内存缓冲区的地址  
  offset：文件中的偏移地址  
  alenp：需要读或写的长度  
  write：为0时表示将文件中的内容读到内存中，为1表示将内存中的内容写到文件中

### vfs数据结构

#### 文件(file)是虚拟文件系统的基本单位，理解文件对理解虚拟文件系统有重要作用。要想理解文件，就要看操作文件的函数是怎么操作文件的。

- file结构体，从进程的角度描述文件系统
  - fd：file结构体是以数组的形式存储在内存中，fd就是file结构体在数组中的编号
  - readable：文件属性，可读
  - writable：文件属性，可写
  - pos：文件中的位置
  - node：文件对应的inode结构体
  - open_count：打开此文件的进程数量
- inode结构体：文件file的抽象表示，是对下层文件系统的封装
- files_struct结构体：与进程相关的文件的信息，是proc_struct结构体中的一项

### vfs操作函数

- **sysfile_close**  
  输入文件的fd，关闭对应的文件。函数执行正确返回0,错误返回对应错误码。
- **sysfile_seek**  
  输入文件的fd,文件内位置pos，条件whence。whence有三种可能的值，LSEEK_SET从文件起始位置，LSEEK_CUR从pos位置，LSEEK_END从文件末尾。如果找到，则会将pos赋值给fd对应的file->pos，并返回0.如果错误会返回错误码。
- **sysfile_read**  
  输入fd,地址base，长度len，将文件中长度为len起始地址file->pos的内容读到内存base处(其中file的地址通过fd确定)。返回实际读取的字节数，如果为负数则为错误码。