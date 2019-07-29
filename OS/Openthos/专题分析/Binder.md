#### 简介

Binder用于进程间通信。java层Binder架构是native层Binder架构的封装。

#### framework层

- frameworks/base/core/java/android/os/IBinder.java

  接口IBinder

- frameworks/base/core/java/android/os/Binder.java

  类Binder

  类BinderProxy

- frameworks/base/core/java/com/android/internal/os/BinderInternal.java

  类BinderInternal

##### Binder框架的初始化

frameworks/base/core/jni/android_util_Binder.cpp

register_android_os_Binder负责Binder框架的初始化。其实就是提前获取JNI层的一些使用信息，通过JNI建立起Java Binder和Native Binder互通的桥梁。

- int_register_android_os_Binder完成Binder类的初始化。

  kBinderPathName为Binder类的路径名

  gBinderOffsets结构体用以保存Binder类在JNI层保存的一些信息，包括类Binder的全局引用、成员函数execTransact的MethodID、成员mObject的FieldID

  用RegisterMethodsOrDie注册Binder类中native函数的实现

- int_register_android_os_BinderInternal完成BinderInternal类的初始化。

  与int_register_android_os_Binder类似

- int_register_android_os_BinderProxy完成BinderProxy类的初始化。

  gErrorOffsets用来和Error类打交道

  gBinderProxyOffsets用来和BinderProxy打交道

  gClassOffsets用来和Class类打交道

  用RegisterMethodsOrDie注册BinderProxy类中native函数的实现

##### Binder的工作原理

##### AIDL的原理