#### 安装
1. 从xilinx官网下载vivado2017.4
2. 解压
3. 运行xsetup
4. 安装在/opt/Xilinx目录下 (或其他用户指定目录)
5. Linux下安装完后需要多一步，安装cable驱动

```bash
cd Xilinx/Vivado/2017.4/data/xicom/cable_drivers/lin64/install_script/install_drivers/
sudo ./install_drivers
```
#### 简介
-  **Vivado**有两种设计流程：**project**模式和**non-project**模式。
-  在运行Vivado GUI的时候，工程文件的路径下会有一个.jou的文件，会自动记录所有GUI操作对应的Tcl脚本，便于查找与使用。
#### 参考资料

[UG892](http://www.xilinx.com/support/documentation/sw_manuals/xilinx2014_2/ug892-vivado-design-flows-overview.pdf) 讲述Vivado设计流程，non-project模式中有针对Tcl脚本的说明
[UG894](http://www.xilinx.com/support/documentation/sw_manuals/xilinx2014_2/ug894-vivado-tcl-scripting.pdf) 讲述如何在Vivado中使用Tcl的文档