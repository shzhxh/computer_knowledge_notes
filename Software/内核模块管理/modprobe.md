#### 简介

添加或删除内核模块。

#### 选项

```
-r, --remove	# 删除模块，也会删除被删模块所依赖的无用模块。
```



#### 示例

```
# LiveSuit安装内核模块
cp awusb.ko /lib/modules/`uname -r`/kernel/
depmod -a
modprobe awusb
```

