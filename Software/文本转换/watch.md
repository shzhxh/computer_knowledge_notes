```
watch [options] <command>	# 周期性执行command，全屏输出，默认周期为2秒
```

#### 选项

```
-d, --differences permanent
-n, --interval seconds	# 执行周期设为seconds秒
-x, --exec		# 命令默认是通过"sh -c"来执行的，这意味着要给命令加引号才能正确执行。而使用此选项，命令将通过"exec"来执行。
```

#### 退出状态

```
0		# 成功执行
1
2
3
4
5
7
8
other
```

