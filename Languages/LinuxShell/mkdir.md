#### 用法

`mkdir [option] directory`

#### 选项

- -m：mode
- -p：如果需要会创建父文件夹
- -v：每创建一个文件夹都会输出一条信息
- -Z：将每个创建的目录的SELinux安全上下文设置为默认类型。
- --context[=CTX]：像`-Z`一样，或者如果指定CTX，则将SELinux或SMACK安全上下文设置为CTX。
- --help：打印帮助信息
- --version：打印版本信息