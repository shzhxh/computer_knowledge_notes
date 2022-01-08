参考：

https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface

http://www.uefi.org/specifications

https://blog.csdn.net/u012459957/article/details/50417302

#### 简介

UEFI是操作系统和平台固件之间的接口，它提供的是启动OS所需要的标准环境。该接口以**数据表**(data tables)的形式存在，包括硬件平台相关的信息、由OS层使用的启动服务调用和运行时服务调用(runtime service calls)。

#### 版本

- 1.x是Intel
- 2.0及以后是标准化了的
- Apple自己玩，是1.x和2.0的混合版。

#### UEFI启动过程

##### 1. SEC(安全验证)

计算机系统加电后进入此阶段。

- 进入固件入口
- 从实模式进入32位平坦模式
- 定位固件中的BFV(Boot Firmware Volume)
- 定位BFV中的SEC映像
- 从32位模式进入64位模式
- 调用SEC入口函数
- 利用CAR(Cache As Ram)技术初始化栈
- 调用PEI入口函数

##### 2. PEI(EFI前期初始化)

将需要传递给DXE的信息组成HOB(Handoff Block)列表，然后将控制权转交到DXE手中。

- 初始化PS(PEI Core Service)
- 调度系统中的PEIM(PEI模块)
- 准备HOB列表
- 调用DXE入口函数

##### 3. DXE(驱动执行环境)

执行大部分系统初始化工作。

- 根据HOB列表初始化系统服务
- 调度系统中的驱动
- 调用BDS入口函数

##### 4. BDS(启动设备选择)

BDS被认为是一种特殊的DXE阶段的应用程序，主要执行启动策略。

- 初始化控制台设备
- 加载必要的设备驱动
- 加载和执行启动项

##### 5. TSL(操作系统加载前期)

OS Loader运行的阶段，OS Loader是一个UEFI应用程序。此时系统资源仍由UEFI内核控制。默认运行OS Loader，但如果用户干预则进入UEFI Shell。当ExitBootServices()服务被调用后，则进入RT阶段。

##### 6. RT(操作系统运行)

OS获得系统控制权。

##### 7. AL(After Life)

为系统固件提供错误处理和灾难恢复机制。

#### 固件位元

UEFI下的所有程序都必须符合UEFI的固件位元/架构。绝大多数的UEFI固件都使用x86_64 UEFI固件。如果`/sys/firmware/efi`存在，表明内核是从UEFI模式启动的，进而表明内核和UEFI具有相同的位元。

#### 关于UEFI的内核配置选项

UEFI系统需要如下配置：

```
CONFIG_RELOCATABLE=y
CONFIG_EFI=y
CONFIG_EFI_STUB=y
CONFIG_FB_EFI=y
CONFIG_FRAMEBUFFER_CONSOLE=y
```

```
CONFIG_EFIVAR_FS=y		# UEFI运行时变量支持(efivarfs文件系统 - `/sys/firmware/efi/efivars`)。诸如`efibootmgr`这样的工具需要它来管理UEFI运行时变量。
```

```
CONFIG_EFI_VARS=n		# UEFI运行时变量支持(旧的efivars sysfs接口 - `/sys/firmware/efi/vars`)。应该禁用此选项，以防止启用efivarfs和sysfs-efivars时出现任何潜在问题。
```

```
CONFIG_EFI_PARTITION=y	# GPT配置选项，UEFI强制支持
```

#### UEFI变量

`efivar --list		#获取变量列表`

##### Linux内核支持的UEFI变量

##### 如何支持UEFI变量

##### 管理UEFI变量的工具

- efivar
- efibootmgr
- uefivars
- efitools
- Ubuntu的固件测试套件

#### UEFI Shell

##### 获取Shell

##### 启动Shell

##### Shell命令

#### UEFI启动介质

##### 从ISO创建启动U盘

- 这种制作启动盘的方式很方便，只要把iso中的内容复制到U盘里即可。但有一点需要注意，U盘的卷标名和配置文件指定的卷标名要一致。

##### 从光盘去除UEFI启动支持

##### 在32bitUEFI上启动64bit内核

#### 在不支持的系统上使用UEFI

##### OVMF

##### DUET

#### 故障排除

