#### run方法

```python
subprocess.run(args, *, stdin=None, input=None, stdout=None, stderr=None, capture_output=False, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None, text=None, env=None, universal_newlines=None)
# args - 要执行的命令
# stdin, stdout, stderr - 子进程的标准输入、输出和错误。可能的值：
#   subprocess.PIPE - 管道
#   subprocess.DEVNULL - os.devnull
#   None - 什么都不做。此为默认值。
#   文件描述符或文件对象
# timeout - 超时时间。如命令执行时间超时，子进程被杀死，并弹出TimeoutExpired异常。
# check - 如设置为True,且退出状态非0，则弹出CalledProcessError异常。
# encoding - 指定编码方式。
# shell - 如设置为True，则通过操作系统的shell执行命令。

示例
subprocess.run(["ls", "-l", "/dev/null"])
```

