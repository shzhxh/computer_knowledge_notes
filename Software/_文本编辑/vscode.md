#### 快捷操作

- 自动换行：搜索"toggle word wrap"。在Ubuntu下为`alt+z`。或者`File > Preferences > Settings`打开用户设置，将`editor.wordWrap`改为`on`。
- 向后跳转：在“Keyboard Shortcuts”搜索"Go Back"可得。在Ubuntu下为`Ctrl+Alt+-`
- 查找文件：搜索"Go to file"可得。在Ubuntu下为`Ctrl+P`
- 跳转到括号的另一边：搜索"go to bracket"可得。在Ubuntu下为`Ctrl+Shift+\`

#### 配置

- rust支持：安装插件rust-analyzer即可。不要安装插件Rust，会冲突。

- 配置C语言的库

  Ctrl+Shift+P调出配置窗口，输入edit，选择“C/CPP:Edit Configurations”，在打开的json文件中“includePath”下添加库文件的地址

#### 问题解决

1. 问题描述：终端中字体显示不正常，字符之间像隔着空格

   解决方法：打开设置-用户-功能-终端，向下翻找到Font Family，设置为"monospace"
