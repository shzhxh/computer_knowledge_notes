```
xorrisofs [options] [-o filename] <pathspecs>
	# <pathspecs>：需要复制进镜像的文件和目录
```

#### 选项

##### 镜像载入

```
```

##### 镜像生成

```
-o, --output <disk_path>	# 设置镜像文件的名字。
```

##### 标准的兼容

```
```

##### 标准的扩展

```
-J, --joliet	# 除了Rock Ridge目录树，再添加Joliet目录树。为了与Windows兼容。
-r, --rational-rock	# 类似于-R。不同之处是：它不保留文件的属主信息，而是把用户id和组id都置为0，并且文件的访问权限是只读。
-R, --rock	# 开启Rock Ridge扩展。对于xorrisofs开说是默认开启的。
```



##### 文件隐藏

```
```

##### ID字符串

```
-V, --volid <text>	# 设置ISO镜像的卷id.
```

##### El Torito可引导镜像

```
```

##### 引导块

```
```

##### 字符集

```
```

##### Jigdo模板提取

```
```

##### 杂项

```
-v, --verbose	# 详细模式
```

#### 示例

```
# 把目录./for_iso的内容写入镜像./image.iso
xorrisofs -r -J -o ./image.iso ./for_iso
```

