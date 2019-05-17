作用：统计给定文件中的字节数，字数或行数。
用法：wc [选项] 文件
	-c 统计字节数
	-w 统计字数
	-l 统计行数
例子：
	wc -l *			//统计当前目录下所有文件的行数
	find . * | xargs wc -l 	//递归统计当前目录下所有文件的行数
	find . * | xargs cat | grep -v ^$ | wc -l	//递归统计当前目录下所有文件的行数，过滤了空行