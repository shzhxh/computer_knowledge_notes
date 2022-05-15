#### 简介

Docker镜像和容器命令行接口(CLI)。是通过CLI与daemon交互的，共有30多个CLI。可以通过`man docker <command>`获取每个命令的具体信息。

#### 安装

```
# 方法一：从系统仓库安装
sudo apt-get install docker docker-engine docker.io	# for ubuntu
sudo pacman -S docker	# for arch

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

##### buildx

```
docker buildx [options] <cmd>	# 使用buildkit来构建
# 选项：
  --builder <string>	# 用来指定builder
# 命令：
  bake
  build [options] <path | url | ->		# 开始一个构建
    -t, --tag <stringArry>	# stringArry的格式是'name:tag'，其中tag是可选的
  create
  du
  inspect
  ls
  prune
  rm
  stop
  use
  version
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
  build [options] <path|url|->	# 从Dockerfile构建镜像
  # --force-rm : 总是移除中间容器
  # -t, --tag <list> : 在'name:tag'格式里的name和可选的tag
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
diff
events
export	# 将一个容器文件系统输出为压缩文件。
history	# 查看某个镜像下的所有层。
import
info
kill	# 杀死一个或多个运行中的容器。
login
logout
logs
pause	# 暂停一个或多个容器中的进程。
port
push
restart	# 重启容器。
rmi		# 移除一个或多个镜像(最顶层)。可以使用-f来删除中间的只读层。
stats
tag
top
unpause
version
wait
```



##### attach

```
docker attach [options] CONTAINER	# 连接到运行中的容器
	# 注：Ctrl+P+Q从容器detach出去

# 选项
  --detach-keys string
  --no-stdin
  --sig-proxy
```

##### build

```
build [options] <Experimental> <PATH|URL|->	# 从Dockerfile创建一个镜像。
	# 读取<PATH>下的Dockerfile(包括子目录)，并把<PATH>下的所有内容发给Docker服务端，由服务端来创建镜像。也可以使用.dockerignore文件来忽略掉路径下的目录和文件。
	
# 选项
-t, --tag[=[]]	# 如果执行成功，则将仓库名应用于得到的镜像。
```

##### commit

```
docker commit [options] <container> [repo[:tag]]	# 从容器创建一个镜像。
# 选项
-a, --author <string>	# 指定作者
-c, --change <list>		# 向要创建的镜像添加Dockerfile指令
-m, --message <string>	# commit信息
-p, --pause				# commit的时候暂停容器(默认为真)
```



##### cp

```
# 是docker container cp的别名
# 注：docker里的路径要使用绝对路径
docker cp [options] <container>:<src_path> <dest_path|->	# 从容器复制到本地
docker cp [options] <src_path|-> <container>:<dest_path>	# 从本地复制到容器
	# 源路径里的-，表示从stdin读取tar文件，然后把它释放到容器里
	# 目的路径里的-，表示把容器里的tar文件释放到stdout

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
docker exec [options] <container> <cmd> [args]	# 在运行的容器中执行一个命令。
# 选项
  -d, --detach		# 在后台运行命令
  --detach-keys <str>
  -e, --env <list>	# 设置环境变量
  -i, --interactive	# 始终保持STDIN打开,即使没有附加
  --privileged
  -t, --tty			# 分配一个伪TTY
  -u, --user <str>
  -w, --workdir <str>
```

##### inspect

```
docker inspect [options] <names|ids>	# 返回docker对象的底层信息。

# 选项
  -f, --format <string>
  -s, --size
  --type <string>
```



##### load

```
docker load [options]	# 从tar文件或STDIN装载镜像

# 选项
 -i, --input <file>	# 读取tar文件(默认STDIN)
 -q, --quiet		# 静默模式
```

##### ps

```
ps		# 列出所有运行着的容器。

# 选项
-a, --all	# 列出所有的容器。(默认列出正在运行的容器)
-f, --filter <cond>	# 按条件<cond>过滤输出
	# <cond>的格式：key=value，如name=abc
--format <str>		# 
-n, --last <int>	# 
-l, --latest		#
--no-trunc			#
-q, --quiet			#
-s, --size			# 
```



##### pull

```
docker pull [options] <name>[:tag|@digest]	# 把镜像拉取到本地。相当于"docker image pull"
# 选项
-a, --all-tags[=false]	# 下载所有标记的镜像
--disable-content-trust[=true]	# 跳过镜像验证
-h, --help[=false]		# 帮助
--platform=""			# 如果服务器适用于多平台，则需设置平台
-q, --quiet[=false]		# 静默模式
```

##### rename

```
docker rename <OLD_NAME> <NEW_NAME>	# 重命名容器
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
-i, --interactive	# 即使没有attached也保持STDIN打开
--mount <mount>	# 把一个文件系统挂载到容器
--name <string>	# 指定容器名
--network <network>	# 把容器连接到一个network
-p, --publish <list>	# 把容器的端口发布到主机
--rm			# 当exit的时候自动删除容器
-t, --tty		# 分配一个伪TTY
-v, --volume <list>	# 绑定并挂载一个卷
```



##### save

```
docker save [options] <images>	# 把镜像保存为tar文件
# 选项
  -o, --output <file>	# 输出到文件(默认STDOUT)
```

##### search

```
docker search [options] <term>	# 在Docker Hub上搜索镜像
    -f, --filter <filter>
    --format <string>
    --limit <int>
    --no-trunc
```



##### start

```
docker start [options] <containers>	# 启动一个或多个已停止的容器。
	-a, --attach	# 附加STDIN/STDERR和forward信号
	--detach-keys <str>	# 
	-i, --interactive	# 附加容器的STDIN
```

##### stop

```
docker stop [options] <containers>	# 停止一个或多个运行中的容器。

# 选项
-t, --time <int>	# 在结束容器之前等待的时间，单位秒
```



#### 配置

```
# 使用代理上网
vim /etc/default/docker # 修改http_proxy
```



#### 使用示例

```
# 使用前准备
sudo usermod -aG docker $USER	# 以非root的权限运行docker
sudo systemctl start docker
sudo systemctl enable docker

# 转移镜像
docker save -o oto8.tar oto_repo	# 把docker镜像oto_repo保存为oto8.tar
docker load -i oto8.tar	# 从oto8.tar载入镜像
docker create -it --name szx-8.1 -v /data/data:/root/szx oto_repo bash	# 从镜像oto_repo创建容器szx-8.1

# 使用docker hub
docker pull alpine:3.12		# 下载镜像
docker create -it --name alpine-3 alpine:3.12	# 从镜像创建容器
docker start -ai alpine-3	# 启动容器

# 容器与主机互传文件
docker cp abc alpine-3:root	# 把文件abc传到容器alpine-3的root目录
docker cp alpine-3:root/abc .	# 把容器alpine-3的abc文件传到当前目录
```

#### 错误分析

1. 问题描述

   脚本script_file.sh在docker内手动运行正常，但使用`docker exec -d docker_name script_file.sh`运行出错。错误原因是：command not found。

   原因分析

   因为在`.bashrc`文件里设置了额外的环境变量，而`docker exec`命令并不会设置额外的环境变量。

   解决方法

   在脚本里使用`source`命令设置所需要的环境变量。
