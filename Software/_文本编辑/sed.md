#### 简介

用于文本过滤和转换的流编辑器(Stream Editor)。流编辑器是对输入流(文件或管道)进行基本的文本转换。


#### 用法

```
sed [option] command [input-file]
```



#### 选项

```
-e <script>或--expression=<script>	# 用指定的script命令处理输入文件。
-f <script文件>或--file=<script文件>	# 用指定的script文件处理输入文件。
-i[SUFFIX], --in-place[=SUFFIX]	# 就地修改文件。把原文件file重命名为file[SUFFIX]，然后把对文件的修改写入到file。
-n或--quiet或--silent		# 不打印过程，只显示结果。
--version					# 打印版本信息。
--help						# 打印帮助文件。
```

#### 命令

  a\text：附加文本text

  c\text：用text替换指定的行  

  d：删除指定的行

  i\text：插入文本text

  p：打印

  s/regexp/replacement/：用replacement替换满足正则表达式regexp的地方

```
s#regexp#replacement#	# 也可以使用'#'号做为分隔字符，此时'/'就无需转义了
```



#### 例子

##### 打印

```bash
# 把文件里从start到end之间的内容打印出来(包含start和end所在的行)
sed -n '/start/,/end/p' file

# 把file1里278-280行的内容重定向到file2里
sed -n '278,280p' file1 > file2
```



##### 删除

  ```shell
# 删除最后一行，将结果重定向到output-file：  
sed '$d' input-file > output-file  
	# "$"的作用请参考正则表达式，意为最后一行。
	
# 删除包含了“abcd”的行
sed -i "/abcd/d" input-file
  
# 删除每行里的前51个字符
sed -i "s/.\{51\}//" file

  ```

##### 替换

```bash
# 将指定目录path下的所有文件里的oldstr替换为newstr
sed -i "s/oldstr/newstr/g" `grep oldstr -rl path`
```



#### 参考文档

- [runoob.com](http://www.runoob.com/linux/linux-comm-sed.html)
- [sed简明教程](https://coolshell.cn/articles/9104.html)