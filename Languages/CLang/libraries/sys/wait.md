```c
/* 作用：挂起调用的线程，直到它的一个孩子结束。
 *　wstatus：如非NULL，则用int类型存储了状态信息
 */
pid_t wait(int *wstatus);

pid_t waitpid(pid_t pid, int *wstatus, int options);

int waitid(idtype_t idtype, id_t id, siginfo_t *infop, int options);

/* 解析wstatus的宏 */
WIFEXITED(wstatus)	//如孩子正常结束则返回true。即调用exit()或_exit()或从main()返回。
```

