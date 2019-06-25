#### 简介

fdisk管理GPT的变种命令共有两个：gdisk是文本界面交互，sgdisk是命令行。它们的作用是把MBR分区表或BSD磁盘标签转换到GPT，或加载一个GPT。

#### 语法

`sgdisk [options] device`

- -o, --clear

  清除所有的分区数据。

- -n, --new=partnum:start:end

  创建一个新的分区。partnum是分区号，start是起始扇区，end是结束扇区。

- -c, --chage-name=partnum:name

  更改分区的GPT名。partnum是分区号，name是分区名。

- -t, --typecode=partnum:{hexcode|GUID}

  更改单个分区的类型代码。partnum代表分区，hexcode或GUID代表类型代码。