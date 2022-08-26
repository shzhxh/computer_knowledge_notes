#### gVisor

##### 复现崩溃


```
# 需要安装bazel,且bazel的版本不得低于4.2.1
# 需安装go,且go的版本不得低于1.16
make	# 构建所有的syzcaller工具
cd $GVISOR
bazel build //runsc	# 构建runsc。注：如fetch失败要通过go env命令走国内的仓库
vim my.cfg	# 生成syz-manager需要的配置文件，注意修改相关的路径
mkdir bundle && vim build/config.json	# 生成配置文件，注意修改相关的路径
sudo runsc -platform=ptrace -file-access=shared -network=host run -bundle ./bundle/ syzkaller	# 运行gvisor
```

##### syzkaller式用法

```
cd $GVISOR
git checkout be6ffa78e4df78df13d004a17f2a8833305285c4	# 切换到指定版本
bazel build --verbose_failures --collect_code_coverage --instrumentation_filter=//pkg/...,-//pkg/sentry/platform,-//pkg/ring0,-//pkg/coverage:coverage //runsc:runsc	# 构建runsc
bazel aquery --collect_code_coverage --instrumentation_filter=//pkg/...,-//pkg/sentry/platform,-//pkg/ring0,-//pkg/coverage:coverage 'mnemonic("GoLink", //runsc:runsc)'
./bazel-out/k8-fastbuild-ST-a2b97ed4b8d6/bin/runsc/runsc_/runsc symbolize -all > symbolize_all_gvisor_be6ffa78e4df78df13d004a17f2a8833305285c4.txt
```

