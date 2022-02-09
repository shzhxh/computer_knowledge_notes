#### 参考文档

码道仕(知乎)：[Python模块Pexpect详解](https://zhuanlan.zhihu.com/p/339070615)

#### 简介

`expect`是一个程序，可以通过脚本和其它程序交互。`pexpect`模块是用python实现了`expect`程序的功能。

`pexpect`的基本工作流程：

1. 用`spawn`执行一个程序。
2. 用`expect`等待关键字。
3. 当匹配到关键字，用`send/sendline`给程序发信息。

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

#### expect()方法

#### send()/sendline()方法

#### interact()方法

#### 示例

```python
#!/usr/bin/env python3.6
#-*- coding:utf-8 -*-

import pexpect

def main(server):
    command = 'ssh -p %s %s@%s' % (server['port'], server['username'], server['hostname'])
    process = pexpect.spawn(command, timeout=30)
    print(f'命令: {command}')
    expect_list = [
        'yes/no',
        'password:',
        pexpect.EOF,
        pexpect.TIMEOUT,
    ]
    index = process.expect(expect_list)
    print(f'匹配到: {index} => {expect_list[index]}')
    if index == 0: 
        process.sendline("yes")
        expect_list = [
            'password:',
            pexpect.EOF,
            pexpect.TIMEOUT,
        ]
        index = process.expect(expect_list)
        print(f'匹配到: {index} => {expect_list[index]}')
        if index == 0:
            process.sendline(server['password'])
            process.interact()
        else:
            print('EOF or TIMEOUT')
    elif index == 1:
        process.sendline(server['password'])
        process.interact()
    else:
        print('EOF or TIMEOUT')

if __name__ == '__main__':
    server = {
        'hostname': '192.168.1.100',
        'port': '22',
        'username': 'admin',
        'password': 'ABuklhsfnVyxI',
    }
    main(server)
```

