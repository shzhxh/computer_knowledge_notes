1. framework/base的jni里的XXXService.cpp里，通过hw_get_module方法获得对应的模块。

2. hw_get_module定义在hardware/libhardware/hardware.c，它调用的是hw_get_module_by_class。

3. hw_get_module_by_class

   1. 拼接出属性名"ro.hardware.*"
   2. 使用property_get，通过属性名找出属性的值
   3. 如果属性值存在，则使用hw_module_exists寻找指定目录下是否存在相应的.so文件。指定目录有/system/lib/hw, /vendor/lib/hw, /odm/lib/hw
   4. 如果存在相应的.so文件，则使用load函数载入模块中的hw_module_t的地址，这样jni就能通过这个接口和hal沟通了。

4. load

   1. dlopen将.so文件加载到内存
   2. dlsym获得hmi的地址
   3. 把模块句柄保存在hmi->dso

