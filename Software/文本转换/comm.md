```
comm [options] <file1> <file2>	# 逐行比较两个排序好的文件。
	# 如不在选项中指定，默认输出三列。第一列为仅在file1中出现的内容，第二列为仅在file2中出现的内容，第三列为两个文件中都有的内容。
```

#### 选项

```
-1							# 仅显示第一列
-2							# 仅显示第二列
-3							# 仅显示第三列
--check-order
--nocheck-order
--output-delimiter=STR		# 用STR分隔各列
--total
-z, --zero-terminated
```

