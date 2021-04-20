```bash
clang [options] <filenames>
```

#### 选项

```
-c	# 只执行预处理、编译、汇编这几个阶段。
-MD	# 写一个depfile，包含用户和系统头文件
-MT <value>	# 指定depfile输出到的文件的名称
-MF <file>	# 把从-MMD, -MD, -MM或-M的depfile输出写入<file>
-o <file>	# 把输出写入<file>
```

