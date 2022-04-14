#### 简介

管理磁盘分区的工具。它相比于`fdisk`和`cfdisk`命令的优势是，支持创建GPT，支持8ZiB的磁盘空间，默认可以有128个分区，使用了CRC校验。

#### 语法

```
parted [options] [device[command[options...]...]]
# options:
  -h, --help	# 显示帮助信息
  -l, --list	# 列出所有块设备的分区信息
  -m, --machine	# 显示机器可分析的输出
  -s, --script	# 不给用户输出提示信息
  -v, --version	# 显示版本号
  -a, --align	# 为新创建的分区设置对齐规则(none,cylinder,minimal,optimal)
# device:
  指定块设备，如果没有指定，将默认使用它找到的第1个块设备
# command [options]
  help [command]	# 打印一般的帮助信息，或command的帮助信息
  align-check type partition	# 检查partition是否满足type的对齐约束，type必须是"minimal"或"optimal"
  mklabel label-type	# 创建新分区表，label-type是分区格式(aix, amiga, bsd, dvh, gpt, loop, mac, msdos, pc98, sun)
  mkpart part-type [fs-type] start end	# 为文件系统fs-type创建分区，分区类型为part-type，从start开始，从end结束。part-type应该是"primary", "logical", "extended".
  name partition name	# 将partition的名称设为name
  print	# 打印分区表
  quit	# 退出
  rescue start end	# 恢复在start和end之间丢失的分区
  resizepart partition end	# 改变partition的end位置
  rm partition	# 删除partition
  select device	# 选择device
  set partition flag state	# 将partition上flag的状态设为state(on,off),flag可以是"boot", "root", "swap", "hidden", "raid", "lvm", "lba", "legacy_boot", "irst", "esp", "palo"。
  unit unit	#
  toggle partition flag	# 切换partition上flag的状态
  version	# 打印版本和版权信息
  
```

#### 命令

```
help		# 如果后面跟着命令，就打印该命令的帮助信息，否则打印通用的帮助信息。
align-check	# 检查指定分区的对齐类型。
mklabel	<label-type>	# 创建disklabel。
	# label-type : aix, amiga, bsd, dvh, gpt, "loop", mac, msdos, pc98, sun
mktable <label-type>		# 创建分区表，作用同mklabel
mkpart [part-type name fs-type] <start> <end>		# 创建分区
	# part-type : "primary", "logical", or  "extended"
	# name : GPT分区表需要
	# fs-type : btrfs,ext[2|3|4],fat[16|32],hfs[+],linux-swap,ntfs, reiserfs, udf, xfs
name		# 为分区命名
print [args]	# 没有参数则显示当前设备的分区表，可选参数如下：
	devices	: 显示所有的块设备
	free	: 显示当前块设备未分配空间的信息
	list,all: 显示所有块设备的分区表
	NUMBER	: 显示NUMBER号分区的详细信息
quit		# 退出
rescue		# 恢复丢失分区
resizepart	# 改变分区大小
rm			# 删除分区
select		# 选择要编辑的设备
disk_set	# 改变设备的FLAG
disk_toggle	# 转换设备的FLAG状态
set			# 改变分区的FLAG
unit		# 设备默认unit
toggle		# 转换分区的FLAG状态
version		# 显示版本号和版权信息
```

#### 用法

```
sudo parted /dev/sda	# 打开parted,选择目标盘sda
(parted) mklabel gpt	# 在当前设备创建分区表
(parted) print			# 查看设备信息
(parted) mkpart primary 0 1396MB	# 创建一个新分区
```

#### 错误解决

1. 34s % 2048s != 0s

   在创建分区的时候产生这个报警，因为没有按照最优的性能对齐。解决方法是用百分比表示。

   ```
   (parted) mkpart primary 0% 40%
   ```

   
