#### 简介

**man**是手册集，通过man可以快速找到权威解释，比在网上搜索要高效准确。

man是通过章节来整理各个手册的。

- 第1章，可执行程序或shell命令
- 第2章，系统调用
- 第3章，库函数
- 第4章，特殊文件
- 第5章，文件格式与协议
- 第6章，游戏
- 第7章，其它
- 第8章，系统管理员命令
- 第9章，内核例程

#### 使用示例

- man的用法：`man man`
- 系统调用write的用法：`man 2 write`
- 把man的内容输出到文本文件： `man * | col -bx > *.txt `

  > col:filter reverse line feeds from input.
  > reverse line feeds:反向换行
  > -b: Do not output any backspaces
  > -x: Output multiple spaces instead of tabs

