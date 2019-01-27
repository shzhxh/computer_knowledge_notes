### 调度的函数

#### wakeup_proc

`void wakeup_proc(struct proc_struct *proc)`

将进程proc的状态设置为PROC_RUNNABLE

#### schedule

`void schedule(void)`

进程调度