由四部分组成：

#### 一 基础镜像

```
FROM <image>[:<tag>]	# 必须在除注释行外的第一行
```



#### 二 维护者

```
Maintainer <docker_user> <email>
```



#### 三 操作指令

```
ADD <src> <dest>	# 复制<src>到容器中的<dest>
COPY <src> <dest>	# 复制<src>到容器中的<dest>
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT <cmd> [param1] [param2]
ENV <key> <value>
EXPOSE <port> [<port>...]
ONBUILD <instruction>
RUN <cmd>	# 在shell中执行/bin/sh -c <cmd>
RUN ["executable", "param1", "param2"]	# 使用exec执行
USER <daemon>
VOLUME <mnt_point>
WORKDIR <dir>	# 为后继的指令指定工作目录
```



#### 四 容器启动时执行的命令

```
# 容器启动时指定了命令，则会覆盖掉CMD命令；如没有指定命令，只有最后一条CMD命令会被执行。即Dockerfile中只应有一条CMD命令。
CMD ["executable", "param1", "param2"]	# 使用exec执行，推荐方式
CMD <cmd> [param1] [param2]	# 在/bin/sh中执行，供需要交互的应用使用
CMD ["param1", "param2"]	# 提供给ENTRYPOINT的默认参数
```

