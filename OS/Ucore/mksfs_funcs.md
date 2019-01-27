本文是对tools/mksfs.c文件的分析，对理解文件系统有帮助。
```
main	// 将指定文件夹中的文件写入指定的镜像
  create_img	// 创建镜像，把文件夹home中的文件加入到镜像文件中
    safe_fchdir	// 将进程的工作目录切换到句柄fd处
    open_dir
    add_entry
       sfs_alloc_ino	// 分配空闲结点
       append_block
         subpath_show
         update_cache
    	search_cache_block
    	alloc_cache_block
    	   hash32
    safe_lstat	// 输入文件名，获取文件属性,返回属性的指针。
    add_dir
      subpath_push
    	safe_strdup
      subpath_pop
    add_file
       open_file
       search_cache_inode
    	 hash64
    add_link
       open_link
    close_sfs
    write_block
    flush_cache_block
    flush_cache_inode
  open_img	// 打开指定名称的文件，将该文件抽象为一个文件系统(可以认为是对create_sfs的封装，获取create_sfs需要的句柄）
    create_sfs	// 通过文件句柄，将该文件创建为一个文件系统(其实就是初始化一个fs结构体）
      safe_fstat	// 通过句柄，获取文件属性
      safe_malloc	// 分配内存空间
      alloc_cache_inode	// 为结点分配内存空间
  static_check	// 类型检查
```