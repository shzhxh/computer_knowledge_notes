#### gVisor


```
# 需要安装bazel,且bazel的版本不得低于4.2.1
# 需安装go,且go的版本不得低于1.16
make	# 构建所有的syzcaller工具
cd $GVISOR
bazel build //runsc	# 构建runsc。注：如fetch失败要通过go env命令走国内的仓库
```

