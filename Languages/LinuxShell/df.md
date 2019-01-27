- df作用：报告文件系统对磁盘空间的占用情况

- 语法：`df [option] [file]`

- 选项：

  ```
  -h	# 以1024的倍数来显示sizes
  -H	# 以1000的倍数来显示sizes
  -k	# --block-size=1K
  -m	# --block-size=1M
  ```

  

- 用法：
  - `df -k`以k为单位显示磁盘占用情况
  - `df -m`以m为单位显示磁盘战胜情况
