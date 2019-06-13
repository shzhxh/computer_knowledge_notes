#### 简介

删除文件或目录，但默认是不删除目录的。注意，`rm`命令删除的文件是有可能被恢复的，如要确保文件不可恢复应使用`shred`命令。

#### 选项

```
-f, --force			# 不给出提示，直接强制删除
-i					# 在删除前给出提示，询问是否删除
-I
--interactive[=WHEN]
--one-file-system
--no-preserve-root
--preserve-root
-r, -R, --recursise	# 递归地删除目录及其内容
-d, --dir			# 删除空目录
-v, --verbose		# 详细描述删除的过程
--help				# 帮助信息
--version			# 版本信息
```



#### 用法示例

* 递归删除目录XXX
  ` rm -rf XXX`

* 反向删除当前目录下除a,b外的所有目录和文件

  `ls | grep -v a | grep -v b | xargs rm -rf`

* 删除以"-"开头的文件

  ```
  # 以文件“-foo”为例
  rm -- -foo		# 方法－
  rm ./-foo		# 方法二
  ```

  
