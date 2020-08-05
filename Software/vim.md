可以通过命令**vimtutor**学习。

#### 语法

```
vim [options] [file ..]		# 编辑指定文件
vim [options] -				# 从标准输入读取文本
vim [options] -t tag		# 编辑tag所在文件
vim [options] -q [errorfile]	# 编辑首先出错的文件
```

#### 选项

```
-d	# 比较多个文件的差异。等价于vimdiff。
-R	# 只读模式。等价于view。
```

#### 使用示例

##### 修改

```
i	//当前字符的前面插入
a	//当前字符的后面插入
ce	//修改到单词末尾(即删除当前光标到单词末尾的字符，并转为插入模式)
cw	//修改单词
c$	//修改到行末
:r		// 在光标处插入内容，可以来自于文件，也可能来自于命令输出
rx	//用x替换当前光标处的字符
```

##### 删除

```
dd	# 删除整行
d$	# 删除到行末(包括本行的最后一个字符)
dw	# 删除一个单词(包括下一个单词之前的字符)
de	# 删除一个单词(包括本单词的最后一个字符)
x	# 删除单个字符
:n,.d	# 删除从第n行到当前行的所有内容
:.,$d	# 删除从当前到最后一行的所有内容
:1,$d	# 删除当前文档的全部内容
```

##### 复制、剪切、粘贴

```
y	# 复制
d	# 剪切
p	# 粘贴
"+y	# 复制到系统剪切板
"+p	# 从系统剪切析粘贴

# 复制粘贴的内容保存在一个叫vim寄存器的地方，可通过如下命令查看寄存器及其内容
:reg
	# 其中""是默认寄存器，最近一次复制、剪切、粘贴操作保存于此
	# "0保存了最近一次复制的内容
	# "1~9分别保存了最近1~9次剪切的内容
	# "a~z供用户使用
	# "+或"*为系统剪切板，跨进程的复制、剪切、粘贴要用到它

# 让vim支持系统剪切板
vim --version | grep clipboard	# 首先查询是否支持，-clipboard不支持，+clipboard支持
sudo apt install vim-gtk		# 对于Ubuntu安装vim-gtk可使vim支持系统剪切板
```

##### 搜索

```
*		# 向后查找光标所在处的单词
#		# 向前查找光标所在处的单词
f<char>	# 向后搜索<char>,光标出现在<char>后面
t<char>	# 向后搜索<char>，光标出现在<char>前面
F<char>	# 向前搜索<char>,光标出现在<char>后面
T<char>	# 向前搜索<char>，光标出现在<char>前面
/	# 搜索，从上向下
?	# 搜索，从下向上
/\C	# 搜索，大小写敏感
/\c	# 搜索，大小写不敏感
# ignorecase和smartcase可以控制搜索时大小写敏感的问题
	:set ignorecase		# 大小写不敏感
	:set ignorecase!	# 大小写敏感
	:set noignorecase	# 大小写敏感
	:set ignorecase?	# 查询是否设置了ignorecase
```

##### 选择

```
v	# 选择，以字符为单位
V	# 选择，以行为单位
```

##### 替换

```
	:s/vivian/sky/	//替换当前行第一个vivian为sky
	:s/vivian/sky/g	//替换当前行所有vivian为sky
	:%s/[a-z]//ig	//删除文档中所有字母，且忽略大小写
	:%s/old/new/gc	//查找全部文件，并提示是否把old替换为new
	:n,$s/vivian/sky	//从第n行到最后一行替换第一个vivian为sky
```

##### 正则表达式

```
	:g/{pattern}/{cmd}	//对所有满足{pattern}的行执行{cmd}命令
	:g/someword/d	//删除所有包含someword的行
```



##### 撤销

```
	u	//撤销
	U	//撤销对整行的修改
	Ctrl+r	//反撤销
```



##### 分屏

```
	:vsp filename	# 纵向分屏，并打开文件filename
	:sp  filename	# 横向分屏，并打开文件filename
	ctrl+w+[hjkl]	# 分屏左下上右切换
```

##### 移动光标

```
	%	//跳转到与光标处括号相匹配的另一个括号
	G	//跳转到文尾
	nG	//跳转到第n行
	gg	//跳转到文首
	j	//向下移动
	k	//向上移动
	h	//向左移动
	l	//向右移动
	ctrl+f	// 向下翻页,front
	ctrl+b	// 向上翻页,back
	ctrl+d	// 向下翻半页,down
	ctrl+u	// 向上翻半页,up
	
	w	# 移动到下个单词的第一个字符
	e	# 移动到下个单词的最后一个字符
	b	# 移到上一个单词
	0	# 移动到行首
	$	# 移动到行尾
```

##### 显示控制

```
	:set number	//显示行号
	:set nonumber	// 不显示行号
	ctrl+g	//显示在文件中的位置与文件信息
```

##### 编辑文件

```
:e[dit] [++opt] [+cmd]	# 编辑当前文件。可用于重新编辑当前文件的情况。
:e[dit]! [++opt] [+cmd]	# 放弃当前缓冲区里的修改，强制编辑当前文件。
	# [++opt]里的opt可以是如下内容：
	ff, fileformat		# 覆盖'fileformat'
	enc, encoding		# 覆盖'fileencoding'
	bin, binary			# 设置'binary'
	nobin, bobinary		# 重新设置'binary'
	bad		# 定义碰到坏字符后采取的行为
	edit	# 
```



##### 文件保存

```
:w[rite] [++opt]	# 把缓冲区的内容写入到当前文件。对于[++opt]来说，仅++bin, ++nobin, ++ff和++enc有效。
:w filename			# 另存为
:w !sudo tee %		# 以超级用户的身份保存文件。%代表当前文件名，tee命令把stdin保存到文件中
```

##### 执行外部命令

```bash
	:!command	// 执行外部命令command
	:%!xxd	//以16进制形式查看文档，%表示文件的所有行，!xxd表示执行xxd命令
	:%!xxd -r	//将修改的内容反写回源文件
```
##### 其它命令

```
;	# 执行上一次操作
```



#### 设置

作用于整个系统的配置文件在`/etc/vimrc`，作用于单个用户的配置文件在`~/.vimrc`。

##### 字符编码

四个与字符编码相关的选项：

| 名称          | 意义                                                         |
| ------------- | ------------------------------------------------------------ |
| encoding      | vim内部使用的编码方式，默认为locale的值，只有在.vimrc中设置它的值才有意义 |
| fileencoding  | 当前编辑的文件的字符编码                                     |
| fileencodings | 一个字符编码的列表，启动时将依据列表自动探测文件的编码方式   |
| termencoding  | vim所工作的终端的字符编码方式                                |



##### 配色方案

可选的配色方案详见`/usr/share/vim/vim81/colors`

```
colorscheme murphy
```

##### 语法高亮

```
syntax on
```

##### 缩进

与自动缩进相关的变量

| 变量名          | 缩写   | 含义                     |
| --------------- | ------ | ------------------------ |
| (no)autoindent  | (no)ai | 是否自动缩进             |
| (no)cindent     | (no)ci | 是否像C语言一样地缩进    |
| (no)smartindent | (no)si | 基于autoindent的一些改进 |

与TAB相关的变量

| 变量名        | 缩写    | 含义                                      |
| ------------- | ------- | ----------------------------------------- |
| tabstop       | ts      | 一个TAB占多少个空格                       |
| shiftwidth    | sw      | 每层缩进的空格数                          |
| (no)expandtab | (no)et  | 是否将tab展开成空格                       |
| softtabstop   | sts     | 按下退格键将删除指定数量的空格            |
| (no)smarttab  | (no)sta | 首行按tab键的时侯，空格数是来自于ts还是sw |

配置示例

```
# 1个tab键变成4个空格
vim ~/.vimrc
	set ts=4
	set et
	set ai
```



#### 常见错误

- 在shell下`Ctrl+s`不是保存文件，而是暂停该终端，此时对终端的所有操作均无效。需要`Ctrl+q`退出当前状态。
- 在shell下Ctrl+z是将该任务挂起了，可以输入`fg`继续之前的编辑。
