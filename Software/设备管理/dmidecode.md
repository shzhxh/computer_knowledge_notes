#### 简介

将DMI table解码为人类可读的形式。DMI table包含了硬件的信息。

DMI table也可以叫SMBIOS table，它们紧密关联，且都是由DMTF开发的。DMI的意思是桌面管理接口(Desktop Management Interface)，SMBIOS的意思是系统管理BIOS.

#### 语法

```
dmidecode [options]
```

#### 选项

```
-d
-h
-H
-q
-s
-t, --type <TYPE>	# 仅显示DMI table里<TYPE>条目的信息。
	# <TYPE>的形式：
	#   DMI类型编号(多个类型编号以逗号分隔)
	#   关键字：bios, system, baseboard, chassis, processors, memory, cache, connector, slot
-u
-V
```

