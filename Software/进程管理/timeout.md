```
timeout [options] <duration> <commnad> [args]	# 运行command命令，在duration时间之后杀死它
```

#### 选项

```
--preserve-status	# 返回<command>的状态。如不用此选项，超时后返回的是124。
--foregroud		# <command>默认是在后台运行的，使用此选项则使<command>在前台运行。这使得<command>可以与TTY交互。
-k, --kill-after=DURATION
-s, --signal=SIGNAL			# 指定要发送的信号
```

