#### 简介

property即属性，由key和value组成，key和value均为字符串。它用来记录系统设置或进程之间的信息交换，是全局可见的。每个进程可以get或set属性。

- ro.* : 只读属性。一旦设置，不能修改。
- persist.* : 此属性的值将同时写入/data/property。
- net.* : net.change将会随此属性自动修改。
- ctrl.start和ctrl.stop : 用来启动和停止服务，启动的结果会放在init.svc.<服务名>属性中。

##### 属性服务的创建

属性服务由init进程创建，其它进程通过属性服务来获取或设置属性。

`system/core/init/init.cpp`

- init进程分配一个共享的内存区域来存储属性

- property_init : 调用__system_property_area_init

- start_property_service : init进程启动属性服务

  用`CreateSocket`函数创建socket资源并绑定

  用listen函数来监听

`system/core/init/property_service.cpp`

- 属性服务调用`__system_property_init`来初始化属性系统的共享内存

- 启动属性服务时，从以下文件加载默认属性值

  /default.prop

  /system/build.prop

  /system/default.prop

  /data/local.prop

- 通过handle_property_set_fd来处理监听到的socket消息

  accept4函数等待建立通信

  getsockopt函数获取套接字相关信息

  socket.RecvUint32函数接收属性设置请求命令

  switch语句来处理接收到的命令，最终都是通过handle_property_set来干活的

##### 属性的操作

`frameworks/base/core/java/android/os/SystemProperties.java`

- 类SystemProperties的get方法用于获取属性值，set方法用于设置属性值。
- 类SystemProperties通过JNI调用实现get和set方法。

`frameworks/base/core/jni/android_os_SystemProperties.cpp`

- JNI函数的具体实现是SystemProperties_set和SystemProperties_get。

`system/core/libcutils/properties.cpp`

- property_init

- property_get
- property_set

`bionic/libc/include/sys/system_properties.h`

`bionic/libc/bionic/system_properties.cpp`

- `__system_property_set`：设置系统属性，将value赋值给key
- `__system_property_get`：不赞成使用。应使用`__system_property_read_callback`
- `__system_property_area_init`：将系统属性的内存区域初始化为只读模式