#### 作用
显示或管理内核的IP路由表。它的主要作用是在使用`ifconfig`命令配置完毕后，通过接口将静态路由配置为特定主机或网络。如果使用了`add`或`del`选项，则会修改路由表。否则，则显示路由表的特定信息。
#### 语法
```shell
  route \[-CFvnee]
  route add
  route del
  route \[-V] \[--version] \[-h] \[--help]
```