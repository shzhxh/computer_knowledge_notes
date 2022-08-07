#### 源码分析

##### 源码下载

```
git clone https://github.com/npat-efault/picocom.git
```

##### main入口

1. 入口main函数在`picocom.c`
2. `parse_args()`解析参数。
3. `establish_signal_handlers()`指定信号处理的函数。
4. `term_lib_init()`初始化库，这样才能使用它。
5. `log_fd = open(opts.log_filename, ...)`打开日志。
6. `tty_fd = open(opts.port, ...)`打开串口设备。
7. `term_set(tty_fd, ...)`设置串口设备的参数。
8. `set_dtr_rts()`设置DTR和RTS的状态。
9. 通过`opts.*`的值判断串口设置是否有冲突。
10. `ler = loop()`进入串口处理的主循环。

11. 结束程序的代码。

##### loop

主体就是一个循环`while(! sig_exit){...}`，在循环里：

1. `r= select(tty_fd+1, ...)`遍历文件符集(rdset, wrset)。
2. `n = read(STI, ...)`读STI看是否有输入。
3. 在一个`for`循环里分析STI的输入，处理特殊字符，使用`tty_q_push()`把处理结果存入`tty_q.buff`。
4. `n = read(tty_fd, ...)`读串口看是否有输入。
5. `n = write(tty_fd, ...)`写串口。

