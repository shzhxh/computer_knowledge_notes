```
# 需要安装bazel,且bazel的版本不得低于4.2.1
bazel build //runsc		# 构建runsc
find -L -type f -name runsc	# 查找生成的runsc
sudo cp <path_to_runsc> /usr/local/bin/
make util-tests
make tests
```

