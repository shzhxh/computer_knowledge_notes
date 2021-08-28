#### make results

`cd src && make results`

#### src: make results

1. build

   ```
   env CFLAGS=-O MAKE="`../scripts/make`" MAKEFLAGS="n" CC="`../scripts/compiler`" OS="`../scripts/os`" ../scripts/build all
   ```

2. config

   ```
   env CFLAGS=-O MAKE="`../scripts/make`" MAKEFLAGS="-k n" CC="`../scripts/compiler`" OS="`../scripts/os`" ../scripts/build opt
   env OS="`../scripts/os`" ../scripts/config-run
   ```

3. results

   ```
   env OS="`../scripts/os`" ../scripts/results
   ```

#### bin/$OS

```
PATH=.:${PATH}; export PATH
export SYNC_MAX
export OUTPUT
lmbench $CONFIG 2>../${RESULTS}
	# $CONFIG : bin/riscv64-linux-gnu/CONFIG.debian
	# $RESULTS : results/riscv64-linux-gnu/debian.0
```

##### bw_file_rd

```
bw_file_rd [-C] [-P <parallelism>] [-W <warmup>] [-N <repetitions>] <size> open2close|io_only <filename>
# bw_file_rd -P 1 <size> open2close|io_only /var/tmp/XXX
# 返回值：一对浮点数，第一个数读取的字节数(m)，第二个读取速率(m/s)
```

测量读文件的带宽

##### bw_mem_cp

测量内存复制的带宽

##### bw_mem_rd

测量读内存的带宽

##### bw_mem_wr

测量写内存的带宽

##### bw_mmap_rd

测量读文件的带宽

##### bw_pipe

测量管道传输数据的带宽

##### bw_tcp

测量通过TCP/IP网络插座传输数据的带宽

##### lat_connect

测量通过TCP/IP进行进程间连接的时延

##### lat_ctx

测量上下文切换的时延

##### lat_fs

测量文件系统创建、删除的时延

##### lat_mem_rd

读内存的时延

##### lat_mmap

mmap和unmmap的时延

##### lat_pagefault

测试一个文件的pagefaulting页的开销

##### lat_pipe

测量通过管道进行进程间通讯的时延

##### lat_proc

创建进程的时延

##### lat_rpc

测量通过Sun RPC进行进程间通讯的时延

##### lat_syscall

测试一些简单的系统调用的时延

##### lat_tcp

测量通过TCP/IP进行进程间通讯的时延

##### lat_udp

测量通过UDP/IP进行进程间通讯的时延

##### lmbench

主控程序

##### lmdd

出于性能和调试的目的统计IO

##### mhz

计算处理器的时钟频率

##### bargraph

把条形图编译为pic

##### graph

把graphs编译为pic