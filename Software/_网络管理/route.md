#### 作用
显示或管理内核的IP路由表。它的主要作用是在使用`ifconfig`命令配置完毕后，通过接口将静态路由配置为特定主机或网络。如果使用了`add`或`del`选项，则会修改路由表。否则，则显示路由表的特定信息。
#### 语法
```shell
  route [-CFvnee]							// 显示路由表的内容
  route add	target							// 添加一个路由
  route del target							// 删除一个路由
  route [-V] [--version] [-h] [--help]
```

#### 选项

```
# 用于显示路由表信息
-A family
-F
-C
-v
-n
-e
-ee

# 用于添加或删除路由
del
add
target		# 目标网络或主机，如果加-net则target是网络，如果加-host则target是主机
  -net		# 详见target
  -host		# 详见target
netmask
gw GW		# 为路由指定网关GW
metric
mss
window
irtt
reject
mod, dyn, reinstate
dev

```

#### 输出

#### 例子