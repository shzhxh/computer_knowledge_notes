#### 简介

以新目录为根目录执行命令

```bash
chroot [options] <root-dir> [<command> [args]]
```

#### 问题解决

1

问题描述：`can't execute '/bin/sh': No such file or directory`

原因分析：要切换过去的目录里没bin/sh

解决方法：`cp /bin/bash bin/sh`
