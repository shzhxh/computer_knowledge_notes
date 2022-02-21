#### 重复执行命令

```bash
# 1 watch
watch -n 1 ls	# 每隔1秒执行一次ls命令

# 2 while
while true; do ls; sleep 1; done	# 每隔1秒执行一次ls命令

# 3 seq
seq 10 | xargs -i ls	# 执行10次ls命令

# 4 for
for i in $(seq 10); do echo $i; sleep 1; done
for ((i=1;i<=10;i++)); do echo $i; sleep 1; done
```

#### 限制命令执行时间

```bash
# timeout
timeout 2 ping www.baidu.com	# 2秒后结束ping命令
```

