- df作用：报告文件系统对磁盘空间的占用情况

- 语法：`df [option] [file]`

- 选项：

  ```
  -B, --block-size=SIZE	# 以SIZE为单位显示结果
  -h, --human-readable	# 以1024的倍数来显示sizes
  -H, --si				# 以1000的倍数来显示sizes
  -k	# 相当于--block-size=1K
  -m	# 相当于--block-size=1M
  
  # SIZE由一个整数和可选的单位组成
  # 以1024为倍数的单位：K,M,G,T,P,E,Z,Y
  # 以1000为倍数的单位：KB,MB,...
  ```

  

- 用法：
  - `df -k`以k为单位显示磁盘占用情况
  - `df -m`以m为单位显示磁盘战胜情况
