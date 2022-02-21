#### 条件表达式

```
# 文件比较
-f文件存在, -d目录存在, -e,文件或目录存在，-s文件存在且非空, -r文件存在且可读, -w, -x

# 字符串比较
==, =, !=, < , >, -n字符串非空, -z字符串为空, =~正则比较(需要加双层中括号)

# 数字比较
-eq, -ne, -lt, -le, -gt, -ge

# 逻辑运算
-a且，-o或，&&且, ||或
	# -a或-o用在[]里
	# &&或||用在[[]]里
```

#### 条件控制

    1. if-then-else

```bash
# 分行写的格式  
  if [ 条件1 ]; 
    then 命令1  # 如果条件1为真，则执行命令1
  elif [ 条件2 ];
    then 命令2  # 如果条件1为假且条件2为真，则执行命令2
  else [ 命令3 ]    # 如果条件1为假且条件2为假，则执行命令3
  fi            # fi意为if语句终止
  
# 写在一行的格式
if [ 1 -ne 0 ]; then echo "not equal"; fi
```

其中，elif和else为可选项。

    2. case

```
  case 值 in
    模式1) 命令1
      ;;
    模式2) 命令2
      ;;

    ...

    *) 命令n
      ;;
  esac
```

将值和每一个模式匹配，若匹配则执行相应命令不再与其它模式匹配，否则执行星号后的命令。

#### 循环

- for

```bash
# 格式一
  for 变量名 in 列表
  do
      命令1
      命令2
      ...
  done
  
# 格式二
  for ((i=0; i<10; i++))
  do
      命令1
      ...
  done
```

一个for循环的例子

```bash
#!/bin/bash

# if a string in file1, then remove it from file2
# $1 : file1
# $2 : file2

for line in `cat $1`
do
	b_name=`basename $line`
	sed -i "/$b_name/d" $2
done
```



- until

```
  until 条件
  do
    命令1
    ...
  done
```

  当条件为真时循环结束，注意测试发生在**循环末尾**。

- while

```
  while 条件
  do
    命令1
    ...
  done
```

- break  

  跳出循环或case语句

- continue  

  跳过当前循环