#### eclipse的使用

* 安装： 
  `sudo apt install eclipse-cdt  `

* 字体设置：  
  window/preferences/general/appearence/colors and fonts/basic/text fonts，点击左边的edit按键。  

* 恢复侧边栏默认布局：  
  window/Reset Perspective

* 跳转到具体实现：  
  Ctrl + 鼠标点击

* 显示行号

  Windows/Preferences/General/Editors/TestEditors，选中右边的`show line numbers`

#### eclipse的错误处理

1. eclipse加载到一半，启动不了，一直提示**loading workspace**。

   解决方法1：我直接把workspace删掉了，缺点是以前加载的项目也被删掉了。

   ```bash
   rm -r PROJ_DIR/.metadate
   ```

2. archLinux里安装eclipse-common启动不了。

   解决方法：直接安装eclipse-cpp。

   `pacman -S eclipse-cpp`

3. 弹出窗口，提示“GC overhead limit exceeded”

   原因：内存不足。通常发生在大型项目中，执行内存密集型操作的时候(比如构建工作区Building workspace)。

   解决方法1：我重启了eclipse，它就好了。

   解决方法2：修改配置文件，调整默认内存的大小。
4. 弹出窗口，提示“An error has occurred.  See the log file”
   ```
   !SESSION Tue May 08 12:29:53 CST 2018 ------------------------------------------
!ENTRY org.eclipse.equinox.launcher 4 0 2018-05-08 12:29:53.347
!MESSAGE Exception launching the Eclipse Platform:
!STACK
java.lang.ClassNotFoundException: org.eclipse.core.runtime.adaptor.EclipseStarter
        at java.base/java.net.URLClassLoader.findClass(URLClassLoader.java:466)
        at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:566)
        at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:499)
        at org.eclipse.equinox.launcher.Main.invokeFramework(Main.java:626)
        at org.eclipse.equinox.launcher.Main.basicRun(Main.java:584)
        at org.eclipse.equinox.launcher.Main.run(Main.java:1438)
        at org.eclipse.equinox.launcher.Main.main(Main.java:1414)
   ```

