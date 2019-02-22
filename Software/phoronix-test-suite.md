#### 安装

```
wget 
sudo dpkg -i *.deb
```

#### 作用

性能测试工具

#### 语法

```
phoronix-test-suite [options]
phoronix-test-suite benchmark [test | suite]
```

#### 选项


##### 系统

```
diagnostics
interactive
php-conf
shell
system-info
system-sensors
```

##### 测试的安装

```
force-install
install
install-dependencies
make-download-cache
remove-installed-test
```

##### 测试

```
auto-compare
benchmark		# 安装并运行指定的测试。
estimate-run-time
finish-run
run
run-random-tests
run-tests-in-suite
stress-run
```

##### 批量测试

```
batch-benchmark		# 和benchmark的作用一样，不过它是批量运行的
batch-install
batch-run
batch-setup			# 设置批量模式，这些设置将依次写入配置文件
default-benchmark
default-run
dry-run
internal-run
```

##### OPENBENCHMARKING.ORG

```
clone-result
list-recommended-tests
make-openbenchmarking-cache
openbenchmarking-changes
openbenchmarking-login
openbenchmarking-refresh
openbenchmarking-repositories
openbenchmarking-uploads
upload-result
upload-test-profile
upload-test-suite
```

##### 信息

```
info
list-all-tests	# 列出OpenBenchmarking.org上所有可用的测试文件
list-*
search
```

##### 创建ASSET

```
build-suite
create-test-profile
debug-*
download-test-files
inspect-test-profile
result-file-to-suite
validate-*
```

##### 结果管理

```
analyze-all-runs
auto-sort-result-file
edit-result-file
extract-from-result-file
merge-results
refresh-graphs
remove-*
reorder-result-file
result-*
show-result		# 显示测试结果
winners-and-losers
```

##### 其它

```
commands
debug-dependency-handler
debug-*
help
version
```

##### 模块

```
list-modules
module-*
test-module
unload-module
```

##### 用户配置

```
enterprise-setup
network-*
user-*
```

##### 网络与图形支持

```
gui
```

