#### 简介

Docker镜像和容器命令行接口(CLI)。是通过CLI与daemon交互的，共有30多个CLI。可以通过`man docker <command>`获取每个命令的具体信息。

#### 安装

```
# 方法一：从系统仓库安装
sudo apt-get install docker docker-engine docker.io

＃ 方法二：从第三方仓库安装
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
engine      # Manage the docker engine
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



##### config

```
docker config <cmd>	# 管理docker的配置
# 命令：
  create	# 创建配置
  inspect	# 显示配置信息
  ls		# 列出配置
  rm		# 删除配置
```



##### container

```
docker container <cmd>	# 管理容器
# 命令
  attach
  commit
  cp		# 详见docker cp
  create	# 详见docker create
  diff
  exec
  export
  inspect
  kill
  logs
  ls
  pause
  port
  prune
  rename
  restart
  rm
  run
  start
  status
  stop
  top
  unpause
  update
  wait
```



##### image

```
docker image <cmd>	# 管理镜像
# 命令，可用docker image <cmd> --help获取详情
  build
  history
  import
  inspect
  load
  ls		# 列出镜像
  prune
  pull		# 把镜像拉到本地
  push		# 
  rm		# 删除镜像
  save
  tag
```



#### 命令

```
commit	# 从容器创建一个镜像。
diff
events
export	# 将一个容器文件系统输出为压缩文件。
history	# 查看某个镜像下的所有层。
import
info
inspect	# 返回docker对象的底层信息。
kill	# 杀死一个或多个运行中的容器。
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
rmi		# 移除一个或多个镜像(最顶层)。可以使用-f来删除中间的只读层。
search
stats
stop	# 停止一个或多个运行中的容器。
tag
top
unpause
version
wait
```



##### attach

```
docker attach [options] CONTAINER	# 连接到运行中的容器

# 选项
  --detach-keys string
  --no-stdin
  --sig-proxy
```

##### build

```
build [options] <Experimental> <PATH|URL|->	# 从Dockerfile创建一个镜像。

# 选项
-t, --tag[=[]]	# 如果执行成功，则将仓库名应用于得到的镜像。
```



##### cp

```
# 是docker container cp的别名
# 注：docker里的路径要使用绝对路径
docker cp [options] <container>:<src_path> <dest_path|->	# 从容器复制到本地
docker cp [options] <src_path|-> <>container>:<>dest_path>	# 从本地复制到容器

# 选项
-a, --archive[=false]	# archieve模式(复制所有的uid/gid信息)
-L, --follow-link[=false]	# 复制<src_path>里的符号链接
-h, --help[=false]		# 帮助信息
```



##### create

```
docker create [options] <image> [cmd] [args]	# 创建容器。实际上是为指定的镜像添加了一个可读写层。

# 选项
  -i, --interactive	# 保持STDIN开启
  --name <string>	# 给容器分配一个名字 
  -t, --tty	# 分配伪TTY
  -v, --volume list	# 本地目录挂载到容器里
```

##### exec

```
docker exec	# 在运行的容器中执行一个命令。
	-i, --interactive	# 始终保持STDIN打开,即使没有附加
	-t, --tty			# 分配一个伪TTY
```



##### load

```
docker load [options]	# 从tar文件或STDIN装载镜像

# 选项
 -i, --input <file>	# 读取tar文件(默认STDIN)
 -q, --quiet		# 静默模式
```

##### rm

```
docker rm [options] <containers> # 删除容器
# 选项
  -f, --force	# 强制删除运行着的容器
  -l, --link	# 删除指定链接(link)
  -v, --volumes	# 删除与容器相关的卷(volumes)
```

##### run

```
docker run [options] <image> [cmd] [args]
	# 在一个新容器里运行命令，相当于create + start
# 选项
-d, --detach	# 在后台运行容器，并打印容器的id
--name <string>	# 指定容器名
--network <network>	# 把容器连接到一个network
-v, --volume <list>	# 绑定并挂载一个卷
```



##### save

```
docker save [options] <images>	# 把镜像保存为tar文件
# 选项
  -o, --output <file>	# 输出到文件(默认STDOUT)
```

##### start

```
docker start [options] <containers>	# 启动一个或多个已停止的容器。
	-a, --attach	# 附加STDIN/STDERR和forward信号
	-i, --interactive	# 附加容器的STDIN
```

#### 配置

```
# 使用代理上网
vim /etc/default/docker # 修改http_proxy
```



#### 使用示例

```
# 使用前准备
sudo usermod -aG docker $USER
sudo systemctl start docker

docker save -o oto8.tar oto_repo	# 把docker镜像oto_repo保存为oto8.tar
docker load -i oto8.tar	# 从oto8.tar载入镜像
docker create -it --name szx-8.1 -v /data/data:/root/szx oto_repo bash	# 从镜像oto_repo创建容器szx-8.1

```

