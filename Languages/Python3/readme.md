#### 正则表示式

#### 第三方库的位置

```
python -m sysconfig | grep platlib	# 系统安装位置
pip install <lib_name>	# 用户安装位置
```



#### 串口交互

1. 串口交互的信息不会缓存，所以要想信息不丢失，必须要有一个守护进程不停地读串口

#### 参数解析

1. 可以通过`sys.argv`处理参数。

2. 可以通过`argparse`模块处理参数。

   ```python
   import argparse
   
   # 创建一个ArgumentParser对象
   parser = argparse.ArgumentParser()
   # 单个的参数如何解析
   parser.add_argument('filenames', nargs='+')
   parser.add_argument('-l', '--lines', type=int, default=10)
   # 把参数转换为对象
   args = parser.parse_args()
   print(args)
   ```

   