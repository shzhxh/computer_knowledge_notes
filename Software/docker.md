#### 简介

Docker镜像和容器命令行接口(CLI)。是通过CLI与daemon交互的，共有30多个CLI。可以通过`man docker <command>`获取每个命令的具体信息。

#### 语法

```bash
docker [options] command [arg...]
docker daemon [-help|...]
docker [--help|-v|--version]
```

#### 选项

```bash
--help	# 打印帮助信息
--config=""	# 
-D,--debug=[true|false]	#
-H,--host=
-l,--log-level="debug|info|warn|error|fatal"
--tls=true|false
--tlscacert= /.docker/ca.pem
--tlscert= /.docker/cert.pem
--tlskey= /.docker/key.pem
--tlsverify=true|false
-v, --version=true|false	# 打印版本信息
```

#### 命令

```
attach
build	# 从Dockerfile创建一个镜像。
commit	# 从容器创建一个镜像。
cp
create	# 创建容器。实际上是为指定的镜像添加了一个可读写层。
diff
events
exec	# 在运行的容器中执行一个命令。
export	# 将一个容器文件系统输出为压缩文件。
history	# 查看某个镜像下的所有层。
images	# 列出所有的顶层镜像。加-a则列出所有的镜像。
import
info
inspect	# 返回docker对象的底层信息。
kill	# 杀死一个或多个运行中的容器。
load
login
logout
logs
pause	# 暂停一个或多个容器中的进程。
port
ps		# 列出所有运行着的容器。加-a则列出所有的容器。
pull
push
rename
restart	# 重启容器。
rm		# 移除一个或多个容器。
rmi		# 移除一个或多个镜像(最顶层)。可以使用-f来删除中间的只读层。
run		# 相当于create + start
save	# 将一个或多个镜像写到一个压缩文件里
search
start	# 启动一个或多个已停止的容器。
stats
stop	# 停止一个或多个运行中的容器。
tag
top
unpause
version
wait
```

#### 运行时执行选项

使用`--exec-opt`标志来定义执行驱动的选项，选项如下：

```
native.cgroupdriver
Client
```

