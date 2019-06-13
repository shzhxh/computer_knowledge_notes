实现Ctrl+Alt+Del组合键的功能

#### 语法

```
ctrlaltdel hard	# 立即重启，不调用sync，不进行任何准备。此为默认选项。
ctrlaltdel soft	# 内核向init进程发送SIGINT信号。
```

