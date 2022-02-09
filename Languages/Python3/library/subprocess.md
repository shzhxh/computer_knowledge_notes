示例：实时读取输出

```python
import subprocess

p = subprocess.Popen("ping www.baidu.com", shell=True, stdout=subprocess.PIPE)

# 方法一
for i in iter(p.stdout.readline, b''):
    print(i.decode())

# 方法二：
flags = fcntl.fcntl(p.output, F_GETFL)
flags = os.O_NONBLOCK
fcntl.fcntl(p.output, F_SETFL, flags)	# 设置flags以消除阻塞
while p.poll() is None:
    print(p.stdout.readline().decode())
```

示例：实时交互

```python
import subprocess as sp

p = sp.Popen("bash", shell=True, stdin=sp.PIPE, stdout=sp.PIPE)

out, _ = p.communicate(input="ls".encode())
p.stdin.flush()
print(out.decode())
```

