正常来考虑，一个不使用系统调用的程序，在被`execve`系统调用执行后，应该不使用任何系统调用。但`musl-gcc`编译出的程序，使用`strace`却看到执行了如下系统调用：

```
set_tid_address
brk
mmap
mprotect
exit_group
```

使用`-static`选项，也有如下的系统调用：

```
set_tid_address
exit_group
```

