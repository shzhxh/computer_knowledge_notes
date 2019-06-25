#### 简介

打印或设备系统的日期和时间

#### 语法

```
date [option] [+FORMAT]
date [-u | --utc | -- universal] [MMDDhhmm[CC]YY[.ss]]
```

#### 选项

```
-d, --date=STRING			# 按照STRING的描述显示时间
--debug
-f, --file=DATEFILE
-I[FMT], --iso-8601[=FMT]	# 以ISO8601的格式输出时期/时间
-R, --rfc-email
--rfc-3339=FMT
-r, --reference=FILE		# 显示FILE的最后修改时间
-s, --set=STRING			# 按照STRING的描述设置时间
-u, --utc, --universal		# 打印或设置协调世界时(UTC)
--help						# 帮助
--version					# 版本
```

#### 格式

控制输出

```
%%		# %本身
%d		# 月份里的日期
%e		# 相当于%_d
%H		# 24小时制的小时(00~23)
%I		# 12小时制的小时(01~12)
%m		# 月份(01~12)
%M		# 分钟(00~59)
%S		# 秒(00~60)
%y		# 年的最后两个数字(00~99)
%Y		# 年

# 默认情况下是以0填充日期的，但可以用如下可选标志跟在"%"后面改变填充效果：
-	# 不填充
_	# 用空格填充
0	# 用0填充
^	# 用大写
#	# 用相反的大小写
```

#### 示例

```
date +%d --date="-1 day"	# 显示前一天的日期
```

