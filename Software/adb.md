#### 简介

Android Debug Bridge，是一个命令行工具，与模拟器或安卓设备交互的桥梁。如果只有一个运行着的模拟器或一个连接着的设备，**adb**会默认与那个设备连接。否则，需要用`-d, -e, -s`选项来指定。

官方文档：http://developer.android.com/tools/help/adb.html

#### 语法

｀adb [选项] 命令｀

#### 选项

```
-a	# 
-d	#
-e	#
-s	#
-p	#
-H	#
-P	#
```

#### 命令

##### 连接

```
devices [-l]　　//显示设备列表

connect host[:port]

```





##### 设备

```
push [-p] local remote	# 将文件或目录复制到设备。-p显示复制过程。
pull					# 从手机中取文件
shell	# 运行远程shell
shell command	# 运行远程shell命令
　adb shell date -s "20120801.120503"   //设置系统日期为2012.08.01.12.05.03
　abb shell rm	// 删除手机中的文件
　adb shell cat
　adb shell getprop
　adb shell stop
　adb shell kill
　adb shell start
　adb shell dumpsys
　adb shell am
　adb shell pm
　adb shell service
　adb shell ime
install [-lrtsdg] <file>	# 将file复制到device并安装它
uninstall					# 卸载
adb get-serialno   //获取设备序列号
```

##### 脚本

##### 网络

#### 环境变量

```
ADB_TRACE
ANDROID_SERIAL
ANDROID_LOG_TAGS
```

#### 常用命令

- **重启adb service**

```
　　adb kill-server
　　adb start-server
```

- **adb 获取root控制**

```
　　adb root
　　adb remount   //将system分区重新挂载为可读写分区
```

- **adb 重启**

```
　　adb shell stop;adb shell start   //重启android
　　adb shell kill -9 system_server/zygote pid   //重启android
　　adb reboot   //重启手机
　　adb reboot bootloader   //重启到刷机模式
　　adb reboot recovery   //重启到recovery，即恢复模式
```

- **adb push 放文件到手机中**

```
　　adb push out/target/product/xx/system/bin/vold /system/bin/   //替换手机中的vold,接收路径最好不要写成system/bin，会有问题
```

- **adb pull 从手机中取文件**

```
　　adb pull /data/data/com.android.providers.settings/databases/settings.db ./   //把手机中设置数据库拿出来
　　adb pull /data/anr/ ./   //取出anr目录下所有文件，包含trace.txt
```

- **adb sync同步**

```
　　首先创建一个目录(如在work下创建sync)，在该目录下创建system/和data目录，目录结构和手机中一致，目录下放置需要同步到手机的文件
　　export ANDROID_PRODUCT_OUT=/home/hunter/work/sync   注：这是在linux环境下
　　adb sync   //这时候文件就会通过push的方式同步到手机，因此需要root权限
```

- **adb logcat 打印log**

```
　　adb logcat   //打印所有log
　　adb shell logcat   //同上
　　adb locat > ./log.txt   //重定向到log.txt
　　adb logcat |tee ./log.txt   //重定向到log.txt，并在控制台输出
　　adb logcat -b main   //过滤main log
　　adb logcat -b events   //过滤event log
　　adb logcat -b radio   //过滤radio log
　　adb logcat -b system   //过滤system log（SLOG）
　　adb logcat -s "TAG"   //过滤某个TAG的log
　　adb logcat *:e   //过滤error log
　　adb logcat *:w   //过滤warn log
　　adb logcat MountService:I Vold:I *:S   //打印TAG为MountService和Vold的Info以上级别的log
　　adb logcat -v time   //显示日期，调用时间，优先级/标记，PID
　　adb logcat -v thread   //仅显示进程：线程和优先级/标记
　　adb logcat -c   //清除log缓存
```

- **adb shell查看进程,内存使用信息**

```
　　VSS - Virtual Set Size 虚拟耗用内存（包含共享库占用的内存）
　　RSS - Resident Set Size 实际使用物理内存（包含共享库占用的内存）
　　PSS - Proportional Set Size 实际使用的物理内存（比例分配共享库占用的内存）
　　USS - Unique Set Size 进程独自占用的物理内存（不包含共享库占用的内存）
```

- **adb shell dumpsys**

```
　　adb shell dumpsys activity   //显示activity相关的信息
　　adb shell dumpsys activity services   //查看service使用细节
　　adb shell dumpsys package   //显示所有程序信息，包含activity,receiver
　　adb shell dumpsys cpuinfo   //显示cpu使用情况信息
　　adb shell dumpsys battery   //显示电池状态
　　adb shell dumpsys window   //显示窗口信息
　　adb shell dumpsys wifi   //显示wifi信息
　　adb shell dumpsys statusbar   //显示状态栏相关的信息
　　adb shell dumpsys gfxinfo com.xxxx.xxx  //dump gfxinfo
　　adb shell dumpsys meminfo   //显示内存使用情况
　　adb shell dumpsys meminfo com.android.settings   //显示设置进程内存使用信息
　　adb shell dumpsys gfxinfo com.android.settings //每一帧花在渲染上的时间估计
```

- **adb无线连接(对于调试otg,sd卡挂载问题很有帮助)**

```
　　1.打开手机wifi热点
　　2.连接数据线，确保可以使用adb，执行adb tcpip 5555
　　3.pc通过无线网卡，如360wifi，连接手机热点
　　4.pc端执行adb connect 192.168.43.1，提示连接成功后，就可以不用数据线也可以debug了
　　5.adb disconnect断开连接
```

- **开启/关闭selinux(android 5.0以上版本快速定位是否是selinux导致问题)**

```
　　adb shell setenforce 1   //开启selinux
　　adb shell setenforce 0   //关闭selinux
```

- **屏幕录制(kk及以后版本才可以)**

```
　　adb shell screenrecord /sdcard/demo.mp4   //录制的视频位于SD卡上
　　adb shell screenrecord  --time-limit 10 /sdcard/demo.mp4   //限制录制时间,单位s
　　adb shell screenrecord --size 1280*720 /sdcard/demo.mp4   //指定视频分辨率大小
```

- **adb install安装应用**

```
　　adb install xx.apk   //安装apk
　　adb install -s xx.apk   //安装到SD卡，需要将安装位置设置为有系统决定或外置SD卡
　　adb install -r xx.apk   //强制安装，如果已经存在就会覆盖安装
　　adb uninstall xxx(package name)   //卸载程序
　　adb shell pm uninstall -k xxx(package name)   //卸载程序，不删除所产生的数据和缓存目录
```

- **adb shell input 命令**

```
　　adb shell input text HelloWorld   //往文本框中输入HelloWorld
　　adb shell input keyevent 26   //输入键值，模仿点击powkey（power key坏了就很有用啦）
　　adb shell input tap 0 0   //模拟点击效果，点击(0,0)位置
　　adb shell input swipe 500 1000 500 0   //模拟滑动效果，从(500,1000)到(500,0)

　　//键值对应如下：
　　KEYCODE_UNKNOWN : 0      KEYCODE_MENU : 1          KEYCODE_SOFT_RIGHT : 2     KEYCODE_HOME : 3 
　　KEYCODE_BACK : 4         KEYCODE_CALL : 5          KEYCODE_ENDCALL : 6        KEYCODE_0 : 7 
　　KEYCODE_1 : 8            KEYCODE_2 : 9             KEYCODE_3 : 10             KEYCODE_4 : 11
　　KEYCODE_5 : 12           KEYCODE_6 : 13            KEYCODE_7 : 14             KEYCODE_8 : 15
　　KEYCODE_9 : 16           KEYCODE_STAR : 17         KEYCODE_POUND : 18         KEYCODE_DPAD_UP : 19
　　KEYCODE_DPAD_DOWN : 20   KEYCODE_DPAD_LEFT : 21    KEYCODE_DPAD_RIGHT : 22    KEYCODE_DPAD_CENTER : 23
　　KEYCODE_VOLUME_UP : 24   KEYCODE_VOLUME_DOWN : 25  KEYCODE_POWER : 26         KEYCODE_CAMERA : 27
　　KEYCODE_CLEAR : 28       KEYCODE_A : 29            KEYCODE_B : 30             KEYCODE_C : 31
　　KEYCODE_D : 32           KEYCODE_E : 33            KEYCODE_F : 34             KEYCODE_G : 35
　　KEYCODE_H : 35           KEYCODE_I : 37            KEYCODE_J : 38             KEYCODE_K : 39
　　KEYCODE_L : 40           KEYCODE_M : 41            KEYCODE_N : 42             KEYCODE_O : 43
　　KEYCODE_P : 44           KEYCODE_Q : 45            KEYCODE_R : 46             KEYCODE_S : 47
　　KEYCODE_T : 48           KEYCODE_U : 49            KEYCODE_V : 50             KEYCODE_W : 51
　　KEYCODE_X : 52           KEYCODE_Y : 53            KEYCODE_Z : 54             KEYCODE_COMMA : 55
　　KEYCODE_PERIOD : 56      KEYCODE_ALT_LEFT : 57     KEYCODE_ALT_RIGHT : 58     KEYCODE_SHIFT_LEFT : 59
　　KEYCODE_SHIFT_RIGHT : 60 KEYCODE_TAB : 61          KEYCODE_SPACE : 62         KEYCODE_SYM : 63
　　KEYCODE_EXPLORER : 64    KEYCODE_ENVELOPE : 65     KEYCODE_ENTER : 66         KEYCODE_DEL : 67
　　KEYCODE_GRAVE : 68       KEYCODE_MINUS : 69        KEYCODE_EQUALS : 70        KEYCODE_LEFT_BRACKET : 71
　　KEYCODE_SLASH : 76       KEYCODE_AT : 77           KEYCODE_NUM : 78           KEYCODE_HEADSETHOOK : 79
　　KEYCODE_FOCUS : 80       KEYCODE_PLUS : 81         KEYCODE_MENU : 82          KEYCODE_NOTIFICATION : 83
　　KEYCODE_SEARCH : 84      TAG_LAST_KEYCODE : 85     KEYCODE_RIGHT_BRACKET : 72 KEYCODE_BACKSLASH : 73
　　KEYCODE_SEMICOLON : 74   KEYCODE_APOSTROPHE : 75
```

- **adb shell am start 启动一个activity**

```
　　adb shell am start -a xxx(action,eg:android.intent.action.VIEW)   //启动activity action
　　adb shell am start -n xxx/.xx(package/.class,eg:com.android.settings/.Settings)   //启动activity
　　adb shell am start -a android.intent.action.CALL -d tel:10086  //给10086拨打一个电话
　　adb shell am start -a android.intent.action.VIEW -d http://www.baidu.com   //打开一个网页
```

- **adb shell am startservice 启动一个服务**

```
　　adb shell am startservice -a xxx(action,eg:android.media.IMediaScannerService)   //启动一个service action
　　adb shell am startservice -n xxx/.xx(package/.class,eg:com.android.providers.media/.MediaScannerService)   //启动一个服务 
```

- **adb shell am broadcast 发送一个广播**

```
　　adb shell am broadcast -a xxx(action,eg:android.intent.action.MEDIA_SCANNER_FINISHED)   //启动一个广播 action
　　adb shell am broadcast -n xxx/.xx   //启动一个广播
　　adb shell am broadcast -a android.intent.action.MEDIA_SCANNER_FINISHED -d file:///system/media   //发送内置SD卡扫描完成广播
```

- **adb shell pm**

```
　　adb shell pm list packages   //列出手机上的所有包
　　adb shell pm clear xxx(包名)   //清除程序数据
　　adb shell pm get-install-location   //获取应用程序安装位置
　　adb shell pm set-install-location 2   //设置应用程序安装位置为外置([0/auto] [1/internal] [2/external])
　　adb shell pm uninstall [-k] xxx(包名)   //保留数据卸载程序
　　adb shell pm disable xxx(包名)   //禁言程序
　　adb shell pm enable xxx(包名)   //启用程序
```

- **adb shell am force-stop 强制关闭一个应用**

```
　　adb shell am force-stop xxx(package name,eg:com.android.deskclock)   //强制关闭一个应用
```

- **adb shell service 显示系统服务**

```
　　adb shell service list   //显示service列表
```

- **adb monkey**

```
　　adb shell monkey -v 500   //产生500次随机事件
　　adb shell monkey -v -p com.android.settings 100   //对设置应用执行100次随机事件
```

- **adb shell ime 输入法管理**

```
　　adb shell ime list   //显示输入法
　　adb shell ime enable ID(com.sohu.inputmethod.sogou/.SogouIME)   //启用输入法
　　adb shell ime disable ID(com.sohu.inputmethod.sogou/.SogouIME)   //禁掉输入法
　　adb shell ime set ID(com.sohu.inputmethod.sogou/.SogouIME)   //设置当前输入法
```


#### 问题汇总
```
问题1： adb server version (40) doesn't match this client (36); killing... 
问题1分析：使用adb version命令，看到Sdk下adb的版本是40，系统里adb的版本是36.我猜是版本冲突，卸载系统里的adb后此问题解决。

问题2：unable to connect to 192.168.0.135:5555: Connection refused
问题2分析：这是由于目标机器上没有开启服务，需要在目标机器的命令行里输入如下命令
  setprop service.adb.tcp.port 5555	// 设置属性，第一个参数是key，第二个参数是value
  stop adbd		
  start adbd	// stop和start两条命令合在一起相当于重启adbd服务
  
问题3: adb connect后，提示offline的问题
问题3分析：我使用的openthos，刚开始没有问题，在openthos休眠再恢复后就提示offline的问题了。adb disconnect后再adb connect，或target和host都重启，问题都未能解决。最终是在target上执行start adbd解决的问题，所以推测是target上adbd出了问题。
```