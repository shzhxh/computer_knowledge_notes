#### 简介

列出指定进程的OOM score和OOM score adjust value，或修改指定进程的OOM score adjust value。

#### 语法

```
choom [options] -p pid
choom [options] -n number -p pid
choom [options] -n number command [args]
```

#### 选项

```
-n, --adjust <num>	# 指定adjust score value
-p, --pid <num>		# 进程ID
```

