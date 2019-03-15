#### 简介

Docker镜像和容器命令行接口(CLI)。是通过CLI与daemon交互的，共有30多个CLI。可以通过`man docker <command>`获取每个命令的具体信息。

#### 安装

```
sudo apt-get remove docker docker-engine docker.io	# 卸载旧版本
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
	# 验证密钥指纹为9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
sudo apt-get install docker-ce
```



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

#### 管理命令

```
builder     # Manage builds
config      # Manage Docker configs
container   # Manage containers
engine      # Manage the docker engine
image       # Manage images
network     # Manage networks
node        # Manage Swarm nodes
plugin      # Manage plugins
secret      # Manage Docker secrets
service     # Manage services
stack       # Manage Docker stacks
swarm       # Manage Swarm
system      # Manage Docker
trust       # Manage trust on Docker images
volume      # Manage volumes
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
	-i, --interactive	# 始终保持STDIN打开,即使没有附加
	-t, --tty			# 分配一个伪TTY
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
	-a, --attach	# 附加STDIN/STDERR和forward信号
	-i, --interactive	# 附加容器的STDIN
stats
stop	# 停止一个或多个运行中的容器。
tag
top
unpause
version
wait
```
