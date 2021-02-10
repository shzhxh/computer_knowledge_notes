#### 基本概念

- 容器：视频文件本身是个容器(container)，里面可以包含视频、音频、字幕等内容。常见的容器格式如MP4、MKV、WebM、AVI等。
- 编码格式：视频和音频都需要经过编码，才能保存成文件。不同的编码格式(CODEC)，有不同的压缩率，会导致文件大小和清晰度的差异。如下视频编码格式有版权但可免费使用H.262、H.264、H.265。如下的视频编码格式无版权VP8、VP9、AV1。常用的音频编码格式有MP3、AAC。
- 编码器(encoders)：实现某种编码格式的库文件。

#### 语法

```
ffmpeg [全局选项] [输入文件选项] -i <输入文件> [输出文件选项] <输出文件>	# 视频转换
```

#### 选项

##### 通用选项

```
-codecs		# 查看FFmpeg支持的编码格式。
-encoders	# 查看FFmpeg已安装的编码器。
-formats	# 查看FFmpeg支持的容器。
```

##### 主选项

```
-i <url>	# 指定输入文件
-c [:steam] <codec>, -codec[:stream] <codec>	# 在输入文件之前时，指定解码器；在输出文件之前，指定编码器。如<codec>为copy，则仅输出不重新编码。
-q[:stream] <q> (output,per-stream)	# 指定输出的品质。<q>的意义依赖于视频所用的编码格式。
-ss <position> (input/output)	# 当作为输入选项，则定位到输入文件的<position>。当作为输出选项，会解码输入文件但放弃结果，直到<position>为止。关于<position>详见ffmpeg-utils手册的Time duration section。
-t <duration> (input/output)	# 当作为输入选项，表示从输入文件读取数据的长度为<duration>；当作为输出选项，表示写入输出文件的数据长度为<duration>。
	# <duration>有两种语法：
	# [-][HH:]<MM>:<SS>[.<m>...]
	# [-]<S>[.<m>...]
-to <position> (output)		# 写入到输出文件，直到<posion>。
-y			# 不进行询问直接覆盖输出文件。
```

##### 视频选项

```
-r [:stream_specifier] <fps> (input/output, per-stream)
	# 设置帧率
-vcodec <codec>		# 设置视频的codec，相当于"-codec:v"
-vn		# 去除音频流
-vframes <number> (output)	# 指定输出的帧数。
```



##### 高级视频选项

##### 音频选项

```
-acodec <codec>		# 设置音频的codec，相当于"-codec:a"
-an		# 去除音频流
```



##### 高级音频选项

##### Subtitle选项与高级Subtitle选项

##### 高级选项

#### 使用示例

```
# 转换编码格式
ffmpeg -i input.file -c:v libx264 output.mp4	# 转换成H.264编码

# 转换容器格式
ffmpeg -i input.mp4 -c copy output.webm			# mp4转webm

# 提取音频
ffmpeg -i input.mp4 -vn -c:a copy output.aac	# 去掉视频，直接复制音频编码

# 添加音轨
ffmpeg -i input.aac -i input.mp4 output.mp4		# 把input.aac和input.mp4合并成output.mp4

# 截图
ffmpeg -i foo.avi -r 1 -s WxH -f image2 foo-%03d.jpeg
ffmpeg -y -i input.mp4 -ss 00:01:24 -t 00:00:01 output_%3d.jpg	# 从1分24秒开始，每隔1秒截1张图
ffmpeg -ss 01:23:45 -i input -vframes 1 -q:v 2 output.jpg	# 从1小时23分45秒截取1帧，-q:v 2表示输出的图片质量，一般是1到5之间(1为质量最高)

# 截取视频
ffmpeg -ss [start] -i <input> -t [duration] -c copy <output>	# 截取视频，从start开始，持续时间duration
ffmpeg -ss [start] -i <input> -to [end] -c copy <output>	# 截取视频，从start开始，到end结束
```



#### 参考资料

[FFmpeg视频处理入门教程——阮一峰](http://www.ruanyifeng.com/blog/2020/01/ffmpeg.html)