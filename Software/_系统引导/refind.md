#### 安装

##### 下载refind

```
# 一、 下载refind
sudo apt install refind	# 安装的时候会询问是否让refind接管系统的启动过程，如果选择了'No'，可以再次执行'dpkg-reconfigure refind'来让refind接管系统的启动过程
	# refind的帮助文档在/usr/share/doc/refind/refind，可从index.html开始
```

##### 使用refind-install命令安装refind

`refind-install`是一个安装脚本，它把相关文件复制到相应位置，并修改固件的NVRAM设置以便在下次启动的时候从refind开始。

```
refind-install [options]	# 把refind安装到ESP并创建一个NVRAM入口
# 选项
--notesp	# 仅用于OS X，把refind安装到OS X的root分区而不是ESP分区。
--usedefault <device-file>	# 把refind安装到指定设备上，要求设备未挂载且是ESP分区。
--root <mount-point>	# 适用于从live CD或应急系统来安装refind。
--ownhfs <device-file>	# 仅用于OS X，把refind安装到HFS+卷而不是OS X boot卷。

--nodirvers	# refind-install会尝试安装一些驱动来读取或启动Linux。此选项的作用是放弃这些驱动的安装。
--alldrivers	# 安装与本机架构相关的所有驱动。有些机器当有任意驱动存在的时候会挂起或不工作，请慎用此选项。

--keepname	# 仅和--shim选项一块用的时候才有效。此选项会保留文件原名，而不是重命名它们。
--shim <shim-filename>	# 把<shim-filename>、MokManager.efi、复制到对应目录，并把shim注册到固件。
--localkeys	# 生成一个新的MOK(Machine Owner Key)，并在安装前对所有refind二进制重新签名。
--encryptkeys	# --localkeys选项的密钥文件是未加密的。使用此选项可对密钥文件加密存储，从而降低启动过程被劫持的风险。
--yes	# 当询问yes/no的时候输出Y，此选项主要适用一些自动化的安装脚本。
```



#### 简单使用

##### 基本特性

##### 调整Boot选项

按下F2键用以显示一个更多选项的菜单，在子菜单里再次按下F2可以编辑这个菜单项。

##### 快捷键

##### 引导传统操作系统

##### 降低启动延迟

#### 配置refind

refind.conf是refind的配置文件，它和其它二进制文件在同一目录中。

##### 隐藏或显示EFI BootLoaders

##### 设置OS图标

##### 调整全局配置

- timeout : 设置等待的时间。如为正数表示等待那么多秒，如为0表示一直等待，如为-1表示不等待直接从默认项启动。
- hideui : 隐藏指定的ui元素。banner隐藏背景图片，label隐藏文字描述和计时器，singleuser隐藏OS X子菜单的单用户选项，safemode隐藏OS X子菜单的启动到安全模式选项，hwtest隐藏Macintosh硬件测试选项，arrows隐藏当OS太多时出现的左右箭头，hints隐藏按键提示。
- icons_dir : 指定自定义图标的目录。
- banner : 指定自定义的背景图片。
- banner_scale ： 指定背景图片的显示方式。noscale为默认值，按原图显示；fillscreen则拉伸图片填满整个屏幕。
- selection_big : 指定用于突出显示OS选择图标的图形文件。
- selection_small ： 指定用于突出显示第二行较小的实用工具的图形文件。
- use_graphics_for :  OS X启动的时候默认不显示任何信息，此选项可以指定OS或bootloader执行这种Mac-style的行为。
- showtools ： 指定第二行显示的工具。reboot重启，shutdown关机。
- dont_scan_dirs：指定一些目录，不在里面扫描bootloaders。此选项有助于将重复的bootloader、驱动或实用工具排除出启动菜单。
- dont_scan_files：指定一些文件不被加到bootloader启动项里。此选项有助于排除支持程序和驱动。
- scan_all_linux_kernels ： 此选项是默认启动的，true、on、1作用是相同的，就是把Linux kernel加载到bootloader列表里。这简化了refind在大多数Linux发行版上的使用。要禁用此功能，必须要指定false、off、0其中之一。
- fold_linux_kernels：此选项是默认启动的，true、on、1作用是相同的，会把给定目录中的所有Linux内核折叠成单个主菜单图标。如果想在主菜单里看到所有内核，必须要将此选项设置为false、off或0。
- csr_values ： 为SIP设置CSR的值，SIP的意思是Apple's System Integrity Protection，这个值是1个字节长度的16进制数(即两位16进制数)。

##### 创建手动启动

每个启动项的基本格式如下：

````
menuentry "name" {
	some keywords
}
````

可用的keywords如下：

| 关键字       | 含义                                                   |
| ------------ | ------------------------------------------------------ |
| menuentry    | 设置菜单项的名称                                       |
| volume       | 指定后续文件访问所在的卷。                             |
| loader       | 指定bootloader。                                       |
| initrd       | 设置Linux内核的initial RAM disk。                      |
| icon         | 指定菜单上显示的图标。                                 |
| ostype       | 当在主菜单按下Insert键时，决定可以在子菜单获得的选项。 |
| graphics     | 图形启动模式的使能(on)或失效(off)。                    |
| options      | 传递给bootloader的选项。                               |
| disabled     | 使此条目失效。相当于把此条目的所有代码都注释掉。       |
| submenuentry | 定义子菜单入口                                         |



##### 创建子菜单

##### 调整默认启动项