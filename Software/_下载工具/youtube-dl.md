#### 安装

```
# 注：youtube-dl只有最新版才有效，而apt命令安装的不是最新版
sudo -H pip install --upgrade youtube-dl
```

#### 用法

```
youtube-dl [options] <URL> [URL...]
```

#### 选项

##### 通用

```
-i, --ignore-errors	# 忽略错误，如忽略下载列表里不存在的文件
```



##### 网络

##### 视频选择

##### 下载

##### 文件系统

```
-o, --output <TEMPLATE>	# 指定输出文件名，详见#输出模板
-c, --continue	# 强制恢复部分已下载的文件，即断点续传。
--no-continue	# 不恢复已下载的文件，即从头开始下载。
```



##### 缩略图

##### 详细/模拟

##### 变通方案

##### 视频格式

```
-f, --format <FORMAT>	# 指定视频的格式，详见#格式选项
-F, --list-formats	# 列出所要获取视频的所有可用格式
--merge-output-format <FORMAT>	# 合并格式(如bestvideo+bestaudio)。<FORMAT>可以是mkv, mp4, ogg, webm, flv.
```



##### 字幕

##### 验证

##### Adobe Pass

##### 后处理

#### 输出模板

#### 格式选项

#### 视频选项