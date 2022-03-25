- df作用：报告文件系统对磁盘空间的占用情况

- 语法：`df [options] [files]`

- 选项：

  ```
  -a, --all	# 所有的文件系统，包括伪文件系统、重复文件系统和不可访问的文件系统
  -B, --block-size=SIZE	# 以SIZE为单位显示结果
  -h, --human-readable	# 以1024的倍数来显示sizes
  -H, --si				# 以1000的倍数来显示sizes
  -k	# 相当于--block-size=1K
  -l, --local	# 仅显示本地文件系统
  -m	# 相当于--block-size=1M
  -T, --print-type		# 打印文件系统类型
  
  # SIZE由一个整数和可选的单位组成
  # 以1024为倍数的单位：K,M,G,T,P,E,Z,Y
  # 以1000为倍数的单位：KB,MB,...
  ```

- 用法：
  - `df -k`以k为单位显示磁盘占用情况
  - `df -m`以m为单位显示磁盘战胜情况

- 错误解决

  - 当使用parted或fdisk扩充磁盘空间后，df命令看到磁盘空间并未改变

    解决方法：使用resize2fs命令调整文件系统的大小
