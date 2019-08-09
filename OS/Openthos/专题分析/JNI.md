#### java层

源码：`media/java/android/media/MediaScanner.java`

##### 1 初始化JNI库

```java
    static {
        System.loadLibrary("media_jni");	// 加载JNI库libmedia_jni.so
        native_init();						// 调用native_init函数(由JNI层实现)
    }
```



##### 2 声明native函数

关键字native表明该函数由JNI层完成

```java
    private native void processDirectory(String path, MediaScannerClient client);
    private native void processFile(String path, String mimeType, MediaScannerClient client);
    private native void setLocale(String locale);

    public native byte[] extractAlbumArt(FileDescriptor fd);

    private static native final void native_init();
    private native final void native_setup();
    private native final void native_finalize();

```



#### JNI层

源码：`media/jni/android_media_MediaScanner.cpp`

##### 1 java层对应函数

```java
static void
android_media_MediaScanner_native_init(JNIEnv *env)
{
    ALOGV("native_init");
    jclass clazz = env->FindClass(kClassMediaScanner);
	... ...

    fields.context = env->GetFieldID(clazz, "mNativeContext", "J");
	... ...
}

```

##### 2 注册JNI函数

- 静态方法

  由虚拟机来完成java层函数和JNI层函数的关联关系，其原理是保存JNI层函数的指针，过程如下：

  ```
  # 1. java代码编译生成class文件
  # 2. 使用命令javah依据class文件生成头文件
  # 3. 实现头文件里声明的JNI层函数
  ```

- 动态方法

  `System.loadLibrary`调用`JNI_OnLoad`(from `/media/jni/android_media_MediaPlayer.cpp`)，`JNI_OnLoad`调用`register_android_media_MediaScanner`实现动态注册，相关源码如下：

  ```java
  # 结构体JNINativeMethod保存了java层和JNI层的关联关系
  # 数组gMethods记录了java层和JNI层的一一对应关系
  static const JNINativeMethod gMethods[] = {
      {
          "processDirectory",		// java中的函数名
          "(Ljava/lang/String;Landroid/media/MediaScannerClient;)V",	// java函数的参数类型和返回值类型
          (void *)android_media_MediaScanner_processDirectory		// JNI层对应的函数指针
      },
  	... ...
  };
  
  # 用于注册native函数
  int register_android_media_MediaScanner(JNIEnv *env)
  {
      return AndroidRuntime::registerNativeMethods(env,
                  kClassMediaScanner, gMethods, NELEM(gMethods));
  }
  ```

##### 3 数据类型

  java数据类型在JNI层中一般会在前面加个j，如jobject(java中的对象)、jint(java中的int类型)、jstring(java.lang.String实例)。

##### 4 JNIEnv

`JNIEnv*`是虚拟机内部的结构体，代表JNI环境，包含了一些JNI系统函数。

#### native层

#### 参考文档

- 《深入理解Android卷1》邓凡平
- [Java Native Interface Specification](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/spec/jniTOC.html)