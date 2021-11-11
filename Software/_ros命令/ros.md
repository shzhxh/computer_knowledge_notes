#### 编译安装

##### 准备

```
# 1 安装bootstrap工具
sudo apt-get install python3-rosdep2 python3-rosinstall-generator python3-vcstool python3-rosinstall build-essential
pip3 install python3-nose	# for test
pip3 install vcstool	# for cmd : vcs

rosdep init
rosdep update
```
##### 安装ros_comm

```bash
# 2 安装
mkdir -p ros_catkin_ws/src && cd ros_catkin_ws
# 选项1:ros_comm，基本库，无GUI
rosinstall_generator ros_comm --rosdistro melodic --deps --tar > melodic-ros_comm.rosinstall
vcs import src < melodic-ros_comm.rosinstall
# 选项2：desktop，建议安装
rosinstall_generator desktop --rosdistro melodic --deps --tar > melodic-desktop.rosinstall
vcs import src < melodic-desktop.rosinstall
# 选项3：desktop_full
rosinstall_generator desktop_full --rosdistro melodic --deps --tar > melodic-desktop-full.rosinstall
vcs import src < melodic-desktop-full.rosinstall
rosdep install --from-paths src --ignore-src --rosdistro melodic -y
	# 安装依赖
```

> ERROR: the following packages/stacks could not have their rosdep keys resolved
> to system dependencies:
> rosconsole: No definition of [log4cxx] for OS version []
> cpp_common: No definition of [boost] for OS version []
> message_filters: No definition of [boost] for OS version []
> rostest: No definition of [boost] for OS version []
> rosbag_storage: No definition of [libgpgme-dev] for OS version []
> class_loader: No definition of [boost] for OS version []
> pluginlib: No definition of [boost] for OS version []
> catkin: No definition of [python-argparse] for OS version []
> xmlrpcpp: No definition of [boost] for OS version []
> rostime: No definition of [boost] for OS version []
> rosbag: No definition of [boost] for OS version []
> rospack: No definition of [boost] for OS version []

```
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
```

> classloader 问题一:
>
> can not find lpthreads 
>
> 解决方法：apt install libevent-pthreads-2.1-7
>
> classloader问题二：
>
> CMake Error at /usr/share/cmake-3.18/Modules/FindPackageHandleStandardArgs.cmake:165 (message):
> Could NOT find Boost (missing: Boost_INCLUDE_DIR thread system)
> Call Stack (most recent call first):
> /usr/share/cmake-3.18/Modules/FindPackageHandleStandardArgs.cmake:458 (_FPHSA_FAILURE_MESSAGE)
> /usr/share/cmake-3.18/Modules/FindBoost.cmake:2177 (find_package_handle_standard_args)
> CMakeLists.txt:12 (find_package)
>
> 解决方法：apt install libboost-all-dev
>
> classloader问题三：
>
> CMake Error at CMakeLists.txt:19 (find_package):
> By not providing "Findconsole_bridge.cmake" in CMAKE_MODULE_PATH this
> project has asked CMake to find a package configuration file provided by
> "console_bridge", but CMake did not find one.
>
> 解决方法：apt install libconsole-bridge-dev
>
> classloader问题四：
>
> CMake Error at /root/Code/ros_catkin_ws/install_isolated/share/cmake_modules/cmake/Modules/FindPoco.cmake:189 (MESSAGE):
> Poco was not found.  Set the Poco_INCLUDE_DIR cmake cache entry to the
> top-level directory containing the poco include directories.  E.g
> /usr/local/include/ or c:\poco\include\poco-1.3.2
> Call Stack (most recent call first):
> CMakeLists.txt:23 (find_package)
>
> 解决方法：apt install libpoco-dev
>
> rospack问题五：
>
> CMake Error at /usr/share/cmake-3.21/Modules/FindPackageHandleStandardArgs.cmake:230 (message):
>
> Could NOT find TinyXML2 (missing: TinyXML2_LIBRARY TinyXML2_INCLUDE_DIR)
>
> 解决方法：apt install libtinyxml2-dev
>
> rosconsole问题六：
>
> CMake Error: The following variables are used in this project, but they are set to NOTFOUND.
> Please set them or make sure they are set and tested correctly in the CMake files:
> LOG4CXX_LIBRARIES
> linked by target "rosconsole-utest" in directory /root/Code/ros_catkin_ws/src/rosconsole
> linked by target "rosconsole-thread_test" in directory /root/Code/ros_catkin_ws/src/rosconsole
>
> 解决方法： apt install liblog4cxx-dev
>
> roslz4问题七：
>
> CMake Error at CMakeLists.txt:13 (message):
> lz4 includes not found
>
> 解决方法：apt install liblz4-dev
>
> roscpp问题八：

```
In file included from /root/Code/ros_catkin_ws/src/ros_comm/roscpp/src/libros/wall_timer.cpp:29:
/root/Code/ros_catkin_ws/src/ros_comm/roscpp/include/ros/timer_manager.h: In member function ‘int32_t ros::TimerManager<T, D, E>::add(const D&, const boost::function<void(const E&)>&, ros::CallbackQueueInterface
*, const VoidConstPtr&, bool)’:                                                                          
/root/Code/ros_catkin_ws/src/ros_comm/roscpp/include/ros/timer_manager.h:344:70: error: ‘_1’ was not declared in this scope                                                                                        
  344 |       waiting_.sort(boost::bind(&TimerManager::waitingCompare, this, _1, _2));                           
解决方法:在wall_timer.cpp里把_1替换为boost::placeholder::_1，把_2替换为boost::placeholders::_2。
原因分析：可能是编译器版本过高，使用_1,_2作为bind占位符的方法已被弃用。请看提示，#pragma message: The practice of declaring the Bind placeholders (_1, _2, ...) in the global namespace is deprecated. Please use <boost/bind/bind.hpp> + using namespace boost::placeholders, or define BOOST_BIND_GLOBAL_PLACEHOLDERS to retain the current behavior.
```

> rosbag_storage问题九：
>
> CMake Error at /usr/share/cmake-3.21/Modules/FindPackageHandleStandardArgs.cmake:230 (message):
> Could NOT find BZip2 (missing: BZIP2_LIBRARIES BZIP2_INCLUDE_DIR)
>
> 解决方法：apt install libbz2-dev
>
> rosbag_storage问题十：
>
> Linking CXX shared library /root/Code/ros_catkin_ws/devel_isolated/rosbag_storage/lib/librosbag_storage.so
>
> /usr/bin/ld: cannot find -lGPGME_LIBRARY-NOTFOUND
>
> 解决方法：apt install libgpgme-dev libgpgme-dev-dbgsym libcrypto++-dev
>
> 修改CMakeFiles/rosbag_storage.dir/link.txt，把-lGPGME_LIBRARY-NOTFOUND删掉
>
> 修改./CMakeFiles/rosbag_default_encryption_plugins.dir/link.txt，把-lGPGME_LIBRARY-NOTFOUND删掉
>
> 问题分析：-lGPGME_LIBRARY-NOTFOUND太奇怪了，不可能有这样的库吧

```
source install_isolated/setup.bash	# 让安装的包生效
```

##### 安装mavros

```bash
# 1 创建工作区
mkdir -p mavros_ws/src && cd mavros_ws
catkin_init_workspace
wstool init src
# 2 安装mavlink
rosinstall_generator --tar --deps --rosdistro melodic mavlink | tee mavros.rosinstall
# 3 安装mavros
rosinstall_generator --tar --deps --upstream --rosdistro melodic mavros | tee -a mavros.rosinstall
# 4 创建工作区
vcs import src < mavros.rosinstall	# 如存在下载错误，则手动下载并解压到src目录
rosdep install --from-paths src --ignore-src --rosdistro melodic -y
	# 安装依赖
```

> ERROR: the following packages/stacks could not have their rosdep keys resolved
> to system dependencies:
> tf2_kdl: No definition of [eigen] for OS version []
> xmlrpcpp: No definition of [boost] for OS version []
> tf_conversions: No definition of [eigen] for OS version []
> rostest: No definition of [boost] for OS version []
> diagnostic_aggregator: No definition of [bondpy] for OS [debian]
> class_loader: No definition of [boost] for OS version []
> ros_comm: No definition of [roslisp] for OS [debian]
> libmavconn: No definition of [boost] for OS version []
> actionlib: No definition of [boost] for OS version []
> tf2_eigen: No definition of [eigen] for OS version []
> catkin: No definition of [python-argparse] for OS version []
> rospack: No definition of [boost] for OS version []
> trajectory_msgs: No definition of [rosbag_migration_rule] for OS [debian]
> pluginlib: No definition of [boost] for OS version []
> cpp_common: No definition of [boost] for OS version []
> orocos_kdl: No definition of [eigen] for OS version []
> rostime: No definition of [boost] for OS version []
> eigen_conversions: No definition of [eigen] for OS version []
> mavros_extras: No definition of [urdf] for OS [debian]
> message_filters: No definition of [boost] for OS version []
> test_rosbag: No definition of [boost] for OS version []
> rosbag: No definition of [boost] for OS version []
> tf2_sensor_msgs: No definition of [eigen] for OS version []
> rosconsole: No definition of [log4cxx] for OS version []
> python_orocos_kdl: No definition of [eigen] for OS version []
> test_mavros: No definition of [control_toolbox] for OS [debian]
> rosbag_storage: No definition of [libgpgme-dev] for OS version []
> mavros: No definition of [boost] for OS version []

```
# 5 安装GeographicLib数据集
./src/mavros/mavros/scripts/install_geographiclib_datasets.sh
# 6 编译
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
# catkin问题一:
CMake Warning at /usr/share/cmake/GMock/GMockConfig.cmake:68 (message):
  Could not detect GTest version, Assuming v1.7.0 (or compatible) and hoping  for the best
  CMake Error at /usr/share/cmake-3.21/Modules/ExternalProject.cmake:2866 (message):                       
  No download info given for 'GMock' and its source directory:
   /usr/src/gmock
   # 解决方法：apt install libgmock-dev libgtest-dev
```



##### 更新

```
mv -i melodic-desktop-full.rosinstall melodic-desktop-full.rosinstall.old
rosinstall_generator desktop_full --rosdistro melodic --deps --tar > melodic-desktop-full.rosinstall
diff -u melodic-desktop-full.rosinstall melodic-desktop-full.rosinstall.old	# 比较差异
vcs import src < melodic-desktop-full.rosinstall
./src/catkin/bin/catkin_make_isolated --install	# 重新构建
source install_isolated/setup.bash
```

#### 初级教程

##### 配置

```
source /opt/ros/noetic/setup.bash
mkdir -p catkin_ws/src && cd catkin_ws
catkin_make	# 在当前目录下创建工作空间
source devel/setup.bash	# 把当前工作空间设置在环境变量的最顶层
echo $ROS_PACKAGE_PATH	# 确认上一条source命令生效
```

##### 文件系统工具

```
rospack find roscpp	# 查找roscpp包所在路径
rospack list		# 列出所有已安装的包
rospack depends roscpp	# 递归检查出roscpp所有的依赖包
rospack depends1 roscpp	# 递归检查出roscpp的一级依赖
roscd roscpp		# 切换到roscpp包所在路径
roscd log			# 如运行过ros程序，则切换到存储ros日志的目录。否则报错。
rosls roscpp		# 显示roscpp包所在路径下的文件
```

##### 创建ros软件包

```
cd catin_ws/src
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp	# 创建ros软件包beginner_tutorials，它依赖于std_msgs、roscpp、rospy。
	# package.xml
```

