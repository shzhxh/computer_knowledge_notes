#### 简介

Pandoc是一个Haskell库，用于将一个标记格式转换为另一个标记格式，也是使用这个库的命令行工具。

#### 语法

```
pandoc [options] [input-file...]	# 输入文件为input-file，如不指定则为stdin，输出默认为stdout
```

#### 选项

##### 通用选项

```
-f, --from, -r, --read		# 指定输入格式
-t, --to, -w, --write		# 指定输出格式
-o, --output				# 指定输出文件,如不指定则默认为stdout.
-v, --version				# 打印出版本
```

##### 读者选项

##### 通用生成器选项

```
-s, --standalone			# 输出相应的header和footer.此选项对于二进制格式来说是自动的,如pdf, docx和odt.但对于文本格式来说则不会默认指定,如HTML, LaTex等.
-V KEY[=VAL], --variable=KEY[:VAL]	# 当在standalone模式呈现文档的时候,为模板变量KEY赋值VAL.
```



##### 影响特殊生成器的选项

```
--latex-engine=pdflatex|lualatex|xelatex	# 生成pdf文件的时候指定latex引擎,默认是pdflatex
```



##### 引用的呈现

##### 在HTML上数学的呈现

##### 包装脚本

#### 模板

#### pandoc的markdown

#### 生成幻灯片

#### 生成EPUBS

#### LITERATE HASKELL支持

#### 语法高亮

#### docx输出里配置样式

#### 配置生成器

#### 用法示例
```shell
# markdown格式转换为docx格式
pandoc -f markdown -t docx ./test.md -o test.docx

# markdown格式转换为pdf格式(可以用`fc-list :lang=zh`命令查询到所有支持中文的mainfont)
pandoc -f markdown test.md -o test.pdf --latex-engine=xelatex -V mainfont="Noto Serif CJK SC"
```

