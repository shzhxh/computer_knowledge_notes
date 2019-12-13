#### 关键字

25个关键字，36个预定义标识符

range关键字用于for循环中迭代数组、切片、通道或集合的元素。

#### 数据类型

布尔型，数字型，字符串型，派生类型(数组，指针，结构体，切片，Map，接口，通道，函数)

```
type(expression)	# 强制类型转换
```



#### 变量

```
# var关键字声明变量
var identifier type	# 指定变量类型
var name = value	# 根据值自行判断
name := value		# 使用:=声明变量

# 声明数组
var variable [size] type	# 指定数组里元素的类型
var variable = [size]type{elements}	# 初始化数组

# 切片：可变长度的数组
var identifier []type	# 通过未指定大小的数组来声明切片
var slice1 []type = make([]type,len)	# 通过make函数创建切片
slice1 := make([]type, len)		# 效果同上
len()获取切片长度，cap()获取切片容量，copy()复制切片，append()向切片追加元素

# 声明指针
var variable *type
# 空指针为nil

# 定义结构体
type struct_type struct{	# 这行里的type是关键字，不是指变量类型
  key1 type
  ...
}
# 声明结构体
var_name := struct_type {value1, value2, ...}
var_name := struct_type {key1:value1, key2:value2, ...}

# Map：无序键值对的集合
var variable map[key_data_type]value_data_type	# 使用map关键字定义map
variable := make(map[key_data_type]value_data_type)	# 使用make函数定义map

# 定义接口
type interface_name interface {
  method_name1 [return_type]
  ...
}
# 通过内置的error接口可以实现简单的错误处理机制

# 通道：用于在两个goroutine之间传递数据
```

#### 常量

```
# const关键字声明常量
cosnt identifier [type] = value
# iota是一个特殊常量，初始值为0，const每增加一行，iota加1
```

#### 运算符

算术，关系，逻辑，位，赋值，其它

#### 条件语句

if, switch, select

#### 循环语句

for(break, continue, goto)

#### 函数

```
func function_name( [parameter list] ) [return_types] {
  ...
}
```

#### 并发

```
go func_name ( params )
```

