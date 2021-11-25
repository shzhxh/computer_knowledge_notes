rt-tests是一个用于测试各种Linux的实时特性的测试套件。包含的程序：

#####  [cyclictest](https://wiki.linuxfoundation.org/realtime/documentation/howto/tools/cyclictest/start)

通过精确测量线程的预期唤醒时间和实际唤醒时间，以提供系统时延的统计信息。这些时延是由硬件、固件和操作系统引起的。

cyclictest通过运行一个非实时的主线程(调度类型SCHED_OTHER)来测试时延，它会启动固定数量的度量线程，这此度量线程有固定的实时优先级(调度类型SCHED_FIFO)。度量线程被在固定间隔内被计时器周期性地唤醒。随后，预期唤醒时间和实际唤醒时间通过共享内存传给主线程。主线程追踪时延的值并打印出它们。

```
cyclictest [options]
# 选项
-a, --affinity[=PROC-SET]	# 指定哪些核来运行线程。不指定[PROC-SET]，则使用所有的核。 
-d, --distance=<DIST>	# 线程间隔之间的距离，单位us，默认值500。当每个核上只有1个线程，建议设置为0.当每个核上有多个线程，用来隔开线程唤醒的时间，这个值会累加到interval的值上。
-D, --duration=<TIME>	# 指定测试运行的时间，可加单位：m分钟，h小时，d天。
-h, --histogram=<US>	# 输出时延直方图(latency histogram)的数据，这些数据可以用来生成一个时延直方图。<US>是要跟踪的最大时延，以毫秒为单位。此选项会使所有线程的优先级相等。
-i, --interval=<INTV>	# 线程间的基本间隔，单位微秒，默认值1000。即线程多久被唤醒1次。
-l, --loops=<LPS>	# 设定循环的次数。默认为0(无限循环)。
-m, --mlockall		# 锁定当前及未来的内存分配，以防止被换页的时候换出
-n, --nanosleep		# 使用clock_nanosleep运行测试，这样精度就更高了。如不用此选项，则使用posix间隔计数器。
-p, --priority=<PRIO>	# 设置第一个线程的优先级。
-q, --quiet		# 仅在退出时打印汇总信息
-S, --smp		# 对SMP系统的标准测试选项。相当于"-t -a -n"，且让所有线程的优先级保持相等
-t, --threads[=num]	# 设置测试线程的数量。如果不指定[num]，则为处理器的核心数。若不使用此选项，则线程数为1.
# 示例
sudo cyclictest -t 5 -p 80 -n	# 5个线程，最高优先级80，无限循环
sudo cyclictest -t 1 -p 99 -n -l 1000 -m -i 2 -h 100	# 1个线程，优先级99，循环1000次，输出时延直方图
sudo cyclictest -m -S -p 80 -i 200 -d 0	# 测试smp系统的时延
```

结果的含意：

| 缩写 | 含义     | 描述                               |
| ---- | -------- | ---------------------------------- |
| T    | Thread   | 线程索引和线程ID                   |
| P    | Priority | RT线程的优先级                     |
| I    | Interval | 度量线程的预期唤醒周期，单位us     |
| C    | Count    | 时延被测量的次数，例如：迭代次数   |
| Min  | Minimum  | 测量到的最小时延，单位us           |
| Act  | Actual   | 最近完成的迭代测量到的时延，单位us |
| Max  | Maximum  | 测量到的最大时延，单位us           |



#####  cyclicdeadline

#####  deadline_test

#####  [hackbench](https://wiki.linuxfoundation.org/realtime/documentation/howto/tools/hackbench)

hackbench用来定位系统中的瓶颈，这些瓶颈应该被消除或优化。

hackbench测试的是内核里的调度器。它还可以通过重复设置和删除线程来对内存子系统进行压力测试。此外，它还可以在一定程度上对进程间通信（如本地的网络插座，管道等）进行压力测试。

它可以测试系统的负载，但不能精确测试应用的负载，因为它不测试与设备的通信。

它创建若干对可调度实体（进程或线程），每对调度实体之间通过网络插座或管道通信，它会记录每对之间来回发送数据的时间。

```
hackbench [options]
# 选项
-p, --pipe	# 使用管道发送数据。（默认用网络插座发送数据。）
-s, --datasize=<SIZE>	# 每条消息的大小。默认100字节。
-l, --loops=<NUM>	# 每个实体发送消息的数量。默认100条。
-g, --groups=<NUM>	# 指定可调度实体的对数。默认10对。
-f, --fds=<NUM>	# 每个实体要用的文件描述符数量。默认40个。
-T, --threads	# 创建的是线程。
-P, --process	# 创建的是进程。默认参数。
--help
# 示例
hackbench	# 创建10对进程，每个进程都通过网络插座发100条数据，每条数据包含100字节，每个进程使用40个文件描述符。
hackbench -p -T	# 创建10对线程，每个线程都通过管道发100条数据，每条数据包含100字节，每个线程使用40个文件描述符。
hackbench -s 512 -l 200 -g 15 -f 25 -P	# 创建15对进程，每个进程都通过网络插座发200条数据，每条数据包含512字节，每个进程使用25个文件描述符。
```



#####  pip_stress

#####  pi_stress

对互斥锁的优先级继承的压力测试。它作为时间优先级任务运行，并启动inversion machine thread groups。每个inversion group都会导致优先级反转条件，这样如果优先级继承不起作用则会死锁。

注：pi_stress线程是作为SCHED_FIFO或SCHED_RR线程运行的，这意味着它们可以使系统关键线程饥饿。建议在pi_stress之前把系统关键线程的调度策略改为SCHED_FIFO，并使用10以上的优先级，以避免pi_stress使这些线程饥饿。

```
pi_stress [options]
# 选项
-i, --inversions=<n>	# 指定反转条件的数量，即所有inversion group可以反转条件的总数。默认是-1代表无限次。
-t, --duration=<n>		# 指定测试运行时间为n秒。
-g, --groups=<n>		# inversion group的数量，默认10.
-d, --debug		# 调试模式，会输出大量信息
-v, --verbose	# 输出详细信息
-s, --signal	# 在接收到SIGTERM信号(Ctrl+C)后结束。默认按下任意键结束。
-r, --rr		# 让inversion threads作为SCHED_RR(round-robin)线程运行。默认是SCHED_FIFO。
-p, --prompt	# 在真正进行压力测试之前进行提示
-u, --uniprocesor	# 用一个处理器运行所有的线程。默认是inversion threads在一个处理器，admin threads在其它的处理器。
-m, --mlockall	# 锁定当前及未来的内存分配，以防止被换页的时候换出
-h
```



#####  pmqtest

#####  ptsematest

#####  queuelat

#####  rt-migrate-test

#####  signaltest

#####  sigwaittest

#####  ssdd

#####  svsematest

#### 编译与安装

```
sudo apt-get install build-essential libnuma-dev	# 安装编译环境和必需的库
git clone git://git.kernel.org/pub/scm/utils/rt-tests/rt-tests.git
cd rt-tests
git checkout stable/v1.0	# master分支不是稳定版，所以要切换到stable分支
make all
make install
```

