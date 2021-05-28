#### 简介

把很多实用程序放到一个文件里。

```bash
busybox <applet> [args]	# 执行busybox里的程序<applet>
<applet> [args]	# 当把busybox符号链接为<applet>，可直接执行它
busybox --install [-s] [DIR]	# 把busybox安装进DIR目录，其实就是把busybox复制进DIR再换个名字。如果使用-s参数则创建的是符号链接。
busybox --list[-full]	# 列出程序名，如加上-full则列出完整的路径。
```

