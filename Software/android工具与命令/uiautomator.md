#### 简介

分析UI控件

#### 语法

`uiautomator command [options]`

#### 命令

```
runtest <jar> <-c|-e class> <CLASS> [options]	# jar文件默认在/data/local/tmp，CLASS是要运行的类。
```

#### 错误解决

- 提示`-Djava.ext.dirs`不受支持

  解决方法：卸载openjdk-11，只保留openjdk-8