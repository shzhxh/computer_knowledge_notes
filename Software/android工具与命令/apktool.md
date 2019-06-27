#### 简介

用于反编译apk文件

#### 语法

```
apktool if | install-framework [options] <framework.apk> # 安装framework
    -p, --frame-path <dir>	# 将framework安装到目录dir
    -t, --tag <tag>			# 
apktool d[ecode] [options]   <file_apk>	# 解码
    -o, --output <dir>	# 指定输出文件夹，默认为*.out
apktool b[uild] [options] <app_path>	# 将解码出的文件再重构为apk文件，注意：要重构文件必须要对文件进行重新签名
    -o, --output <apk_name>	# 指定apk文件名，默认为dist/name.apk
```

