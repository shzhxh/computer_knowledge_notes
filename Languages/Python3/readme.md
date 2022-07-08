#### 串口交互

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

   