#### Framework层

`android.media` API

##### MediaServer

代码位置：`frameworks/av/media/mediaserver/main_mediaserver.cpp`

用C++写的一个可执行程序(虽然不是用Java写的，但Android是基于Linux内核的，所以这个程序也是可以运行的)。

1. 获得一个ProcessState的实例
2. 调用defaultServiceManager，向ServiceManager注册服务，得到IServiceManager
3. 初始化多媒体系统的MediaPlayer服务
4. 创建一个线程池？
5. 把自己加入线程池？

#### Binder IPC

`frameworks/av/media/libmedia/I*`

#### Native层

- Stagefright

  媒体播放引擎，内置编解码器OpenMax。

- MediaPlayerService

  在framework/av/media/libmediaplayerservice/目录

#### 参考资料

- [Media_AOSP](https://source.android.google.cn/devices/media)
- 《深入理解Android卷一》——邓凡平

