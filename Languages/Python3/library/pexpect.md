#### spawn类

```python
pexpect.spawn(cmd,...)	# 执行cmd命令
    # timeout=<TIMEOUT> : 单位为秒，用于引发expect()超时。默认30。如为None，expect()有可能一直阻塞直到匹配。
    # encoding=<ENCODING>	// python3需要向spawn传递此参数
	# 返回值：cmd的handle

#== 成员 ==
logfile		# 指定日志文件。如设置为sys.stdout则输出到stdout。如设置为None则不输出日志。默认为None。

#== 方法 ==
expect(pattern,...)	# 等待pattern配置stream。
```

