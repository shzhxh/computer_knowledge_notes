#### 前提条件

##### ubuntu18.04

##### minigui

```
# 0. 准备
sudo apt install git gcc binutils autoconf libtool make cmake pkg-config	# 工具
sudo apt install libgtk2.0-dev libjpeg-dev libpng-dev libfreetype6-dev libsqlite3-dev libxml2-dev libssl1.0-dev		# 库

# 1. 获取编译脚本
git clone https://github.com/VincentWei/build-minigui-4.0.git

# 2. 获取源代码
cd build-minigui-4.0
./fetch-all.sh	# 在config.sh修改"git@"为“https://”，且把"VincentWei/"里的反斜杠去掉

# 3. 编译工具和第三方库
./build-deps.sh
	# 在编译harfbuzz出错，提示没有"ragel"，需要"sudo apt install ragel"
	
# 4. 编译全部代码
./build-all.sh

# 5. 运行显示程序
sudo ldconfig	# 刷新共享库的缓存
mguxdemo	# 界面闪退，提示"SQLITE_OK == ret"错误。

# 6. 更新与重新安装
./update-all.sh		# 更新
./clean-build-all.sh	# 重新安装
```

#### 编译过程

##### hfcl

```
git clone https://github.com/FMSoftCN/hybridos.git
cd hybridos/device-side/hfcl
./autogen.sh
./configure
make
	# 如编译csscomputed.cc出现错误，可删除src/css/Makefile里CPPFLAGS变量的-Werror
sudo make install
```

##### samples

```
cd device-side/samples
./autogen.sh
./configure
make
	# 如提示...vile/view.h里找不到"../drawable/drawable.h"，则将hfcl/include下的drawable目录复制到/usr/local/include/hfcl目录
	# 如提示view.h里找不到“view/*tview.h”，则直接在/usr/local/include/hfcl/view目录下创建对应的空文件
	# 如提示关于Style未定义的问题，可在/usr/local/include/hfcl/resource/respackage.h里添加一个include语句包含"../drawable/drawable.h"
	# 如提示"/usr/bin/ld: cannot find -lefence"，则需要sudo apt install electric-fence
```



