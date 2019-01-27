可以通过命令vimtutor学习。

#### 语法

```
       vim [options] [file ..]
       vim [options] -
       vim [options] -t tag
       vim [options] -q [errorfile]
```

#### 选项

```
-d	# 比较多个文件的差异。就像vimdiff那样工作。
```



#### 命令模式

```bash
	:s/vivian/sky/	//替换当前行第一个vivian为sky
	:s/vivian/sky/g	//替换当前行所有vivian为sky
	:%s/[a-z]//ig	//删除文档中所有字母，且忽略大小写
	:%s/old/new/gc	//查找全部文件，并提示是否把old替换为new
	:n,$s/vivian/sky	//从第n行到最后一行替换第一个vivian为sky
	
	:vsp filename	# 纵向分屏，并打开文件filename
	:sp  filename	# 横向分屏，并打开文件filename

	:n,.d	//删除从第n行到当前行的所有内容
	:1,$d	//删除全部内容
	:set number	//显示行号
	:set nonumber	// 不显示行号
	:w filename	//另存为
	:w !sudo tee %	// 以超级用户的身份保存文件。%代表当前文件名，tee命令把stdin保存到文件中
	
	:r		// 在光标处插入内容，可以来自于文件，也可能来自于命令输出
	
	:!command	// 执行外部命令command
	:%!xxd	//以16进制形式查看文档，%表示文件的所有行，!xxd表示执行xxd命令
	:%!xxd -r	//将修改的内容反写回源文件
	
	:g/{pattern}/{cmd}	//对所有满足{pattern}的行执行{cmd}命令
	:g/someword/d	//删除所有包含someword的行
```
#### 一般模式
```bash
删除
	dd	//删除整行
	d$	//删除到行末(包括本行的最后一个字符)
	dw	//删除一个单词(包括下一个单词之前的字符)
	de	//删除一个单词(包括本单词的最后一个字符)
	x	//删除单个字符
修改
	i	//当前字符的前面插入
	a	//当前字符的后面插入
	ce	//修改到单词末尾(即删除当前光标到单词末尾的字符，并转为插入模式)
	cw	//修改单词
	c$	//修改到行末
	
	v	//选择
	V	//选择一行	
	y	//复制
	d	//剪切
	p	//粘贴
	rx	//用x替换当前光标处的字符
	
	u	//撤销
	U	//撤销对整行的修改
	Ctrl+r	//反撤销
搜索	
	/	//搜索，从上向下
	？	//搜索，从下向上
	%	//跳转到与光标处括号相匹配的另一个括号
移动光标	
	G	//跳转到文尾
	nG	//跳转到第n行
	gg	//跳转到文首
	j	//向下移动
	k	//向上移动
	h	//向左移动
	l	//向右移动
	w	//移动到下个单词的第一个字符
	e	//移动到下个单词的最后一个字符
	0	//移动到行首
```
#### 组合键
如果不小心按下Ctrl+s暂停该终端，需要Ctrl+q退出当前状态。

```
	ctrl+g	//显示在文件中的位置与文件信息
```

