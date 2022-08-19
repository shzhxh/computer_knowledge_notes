#### 安装

```bash
wget https://go.dev/dl/go1.19.linux-amd64.tar.gz	# 下载
tar xf go1.19.linux-amd64.tar.gz -C /usr/local		# 解压前要确保/usr/local/go目录不存在
vim ~/.profile	# 添加如下内容
	export PATH=$PATH:/usr/local/go/bin
```



#### 用法

```
go <command> [arguments]
```

#### 命令

```
clean
doc
env
fix
fmt
get
install
list
test
tool
version
vet
```

##### build

```
go build [-o output] [build flags] [packages]	# 编译包和依赖
# 选项
```

##### run

```
run [build flags] <gofiles> [arguments]
	# 编译和运行Go程序。关于[build flags]请查询"go-build"。
```

#### 问题解决

##### i/o timeout

```
go env -w GO111MODULE="on"
go env -w GOPROXY="https://goproxy.cn,direct"
```

