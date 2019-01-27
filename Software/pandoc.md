#### 简介

Pandoc是一个Haskell库，用于将一个标记格式转换为另一个标记格式，也是使用这个库的命令行工具。

#### 语法

`pandoc [options] [input-file]...`

#### 用法示例
```shell
# markdown格式转换为docx格式
pandoc -f markdown -t docx ./test.md -o test.docx
```

