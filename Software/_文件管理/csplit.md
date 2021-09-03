#### 简介

把大文件分割为小文件，每个文件的名字类似于xx00, xx01，...

#### 语法

```
csplit [options] <file> <patterns>	# 按<patterns>的规定分割<file>
```

#### 选项

```
-b, --suffix=FMT	# 后缀格式不使用默认的%02d，而使用FMT，FMT的格式类似于C语言的printf
-f, --prefix=PRE	# 前缀不使用默认的'xx'，而使用PRE
-k
--suppress-matched
-n, --digits=N		# 分割后的文件名中数字的个数默认为2，在此可指定为N
-s					# 静默模式
-z
```

#### **模式**

```
# OFFSET是一个正整数或负整数
INTEGER				# 复制到指定行号
/REGEXP/[OFFSET]	# 复制到满足正则表达式REGEXP的行
%REGEXP%[OFFSET]	# 从当前位置到满足正则表达式REGEXP的行，其中的所有内容全部跳过
{INTEGER}			# 按照之前的模式重复分割INTERGER次
{*}					# 按照之前的模式重复分割直到文件末尾
```

#### 示例

```
# 以某行中存在qemu-system-x86_64为条件，分割test.log。分割后的文件以abc开头。
csplit -f abc test.log /qemu-system-x86_64/ {*} 
```

