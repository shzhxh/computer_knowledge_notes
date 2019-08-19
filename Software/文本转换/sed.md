#### 简介

用于文本过滤和转换的流编辑器(Stream Editor)。流编辑器是对输入流(文件或管道)进行基本的文本转换。


#### 用法

```
sed [option] command [input-file]
```



#### 选项

  -e \<script>或--expression=\<script>：用指定的script命令处理输入文件。

  -f <script文件>或--file=<script文件>：用指定的script文件处理输入文件。

  -n或--quiet或--silent：不打印过程，只显示结果。

  --version：打印版本信息。

  --help：打印帮助文件。

#### 命令

  a\text：附加文本text

  c\text：用text替换指定的行  

  d：删除定位行

  i\text：插入文本text

  p：打印

  s/regexp/replacement/：用replacement替换满足正则表达式regexp的地方

#### 例子

  ```shell
# 保存sed输出，即将结果重定向到output-file:  
sed 'sed-command' input-file > output-file
    
# 删除最后一行，将结果重定向到output-file：  
sed '$d' input-file > output-file  
# "$"的作用请参考正则表达式，意为最后一行。
  
# 将指定目录path下的所有文件里的oldstr替换为newstr
sed -i "s/oldstr/newstr/g" `grep oldstr -rl path`

# 
sed -n 's/ magicmagicmagic/ /p' file1 > file2
  ```

#### 参考文档

- [runoob.com](http://www.runoob.com/linux/linux-comm-sed.html)