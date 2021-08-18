#### 简介

逐行比较文件

#### 语法

```
diff [options] <files>
```

#### 选项

```
-r, --recursive		# 递归比较两个目录里的所有文件
-N, --new-file		# 将不存在的文件当成空文件
--suppress-common-lines	# 与-y联用，表示只输出内容不同的列。
-u, -U NUM, --unified[=NUM]	# 以合并的方式来显示文件内容的不同
-W, --width=<NUM>	# 允许输出列数最高为<NUM>，默认130。
-y, --side-by-side	# 输出成两列
```

#### 用法

```
diff -ruN lab1/kern lab2/kern > lab2.patch	# 递归比较lab2/kern文件夹与lab1/kern文件夹中所有文件不同之处，结果输出到lab2.patch文件中。注：生成的.patch文件是与patch命令配合使用的
```

