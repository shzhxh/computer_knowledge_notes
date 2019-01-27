#### 语法

```
tar 操作模式 [选项] [参数]
```



#### 操作模式

- -A 附加tar文件
- -c  创建
- -d archive与文件系统比较 
- --delete 从archive中删除
- -r 附加
- -t 列出archive的内容
- --test-label 测试archive卷标并退出
- -u 更新
- -x 解压
- --show-defaults 
- --help 帮助
- --usage 打印可用操作
- --version 打印版本信息

#### 选项
- -f 使用archive文件或设备。

#### 用法示例

```
tar cf abc.tar ./abc	# 给abc目录创建归档文件abc.tar
tar xf abc.tar -C ~/	# 将abc.tar解压到用户目录
```

