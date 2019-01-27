- ucore的实现步骤：
  1. proj1 显示字符的bootloader
  2. proj2 将内核载入内存的bootloader
  3. proj3 显示字符的内核
  4. proj3.1
  5. proj4 管理中断、键盘和时钟
  6. proj4.1 内核态和用户态的切换,用中断实现系统调用
  7. proj4.2 增加gdb remote server/stub，实现gdb远程调试
  8. proj4.3 增加硬件breakpoint和watchpoint，从而具有内部debugger功能
  9. proj5 分页机制
  10. proj5.1 连续内存分配算法
  11. proj5.2 伙伴分配算法
  12. proj6 slab内存分配算法
  13. proj7 虚拟内存管理框架
  14. proj8 页替换算法
  15. porj9 内核函数map,unmap,dup,exit
  16. proj9.1 实现share memory机制
  17. proj9.2 实现COW机制
  18. proj10 实现PCB
  19. proj10.1 用户进程管理框架
  20. proj10.2 进程管理相关的系统调用
  21. proj10.3 heap的系统调用sys_brk
  22. proj10.4 函数sleep，timer功能支持
  23. proj11 线程方式实现swap机制
  24. proj12 实现用户态线程
  25. proj13 先来先服务调度算法
  26. proj13.1 轮转调度算法
  27. proj13.2 多级反馈队列调度算法
  28. proj14 内核中的信号量机制
  29. proj14.1 用户态进程/线程的信号量机制
  30. proj14.2 信号量等待中的超时判断机制
  31. proj15 事件的IPC机制
  32. proj16 邮箱的IPC机制
  33. proj16.1 管程和条件变量
  34. proj17 实现vfs框架
  35. proj17.1 实现匿名管道和有名管道
  34. proj18 增加simple filesystem
  35. proj18.1 增加链接相关的系统调用和内核函数
  36. proj18.2 增加exec函数
  37. proj18.3 exec函数的参数功能
  38. proj19 实现shell
