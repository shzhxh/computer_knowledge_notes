[Linker Scripts](https://sourceware.org/binutils/docs/ld/Scripts.html)

#### 脚本格式

链接脚本由一系列命令组成。

#### 脚本命令

##### ENTRY

ENTRY(SYMBOL) ：将符号SYMBOL的值设置成入口地址。

##### INCLUDE

INCLUDE filename：把其它链接脚本filename链接到当前脚本中。

##### OUTPUT_ARCH

设置输出文件的机器架构

##### MEMORY

```
MEMORY {
NAME1 [(ATTR)] : ORIGIN = ORIGIN1, LENGTH = LEN1
NAME2 [(ATTR)] : ORIGIN = ORIGIN2, LENGTH = LEN2
…
}
```

NAME:存储区域的名字。

ATTR:该存储区域的属性。R只读，W读写，X可执行，A可分配，I已初始化，L同I，!不满足。

ORIGIN:区域的开始，可简写成org或o.

LENGTH:区域的大小，可简写成len或l.

##### PHDRS

```
PHDRS
{
NAME TYPE [ FILEHDR ] [ PHDRS ] [ AT ( ADDRESS ) ]
[ FLAGS ( FLAGS ) ] ;
}
```

NAME:程序段名。

TYPE:PT_NULL程序段未被使用，PT_LOAD该程序段在运行时被加载，PT_DYNAMIC该程序段包含动态连接信息，PT_INTERP该程序段内包含程序加载器的名字，PT_NOTE该程序段内包含程序的说明信息，PT_SHLIB保留的程序头类型，PT_PHDR该程序段包含程序头信息，EXPRESSION 表达式值。

FILEHDR关键字，表示该段包含ELF文件头信息。

PHDRS关键字，表示该段包含ELF程序头信息。

AT(ADDRESS)属性定义该程序段的加载位置(LMA)。

FLAGS标志，该标志用于设置程序段描述的p_flags域。

##### SECTIONS

SECTIONS命令告诉链接器如何把输入文件的sections映射到输出文件的sections，且会说明输出文件的sections放在内存中的什么位置。SECTIONS命令由多个sections-command组成，sections-command是如下四种之一：

1. ENTRY命令，详见ENTRY命令

2. 符号赋值语句，详见赋值语句

3. 输出section的描述

   ```
   SECTION-NAME [ADDRESS] [(TYPE)] : [AT(LMA)]
   {
   OUTPUT-SECTION-COMMAND
   OUTPUT-SECTION-COMMAND
   …
   } [>REGION] [AT>LMA_REGION] [:PHDR HDR ...] [=FILLEXP]
   ```

   SECTION-NAME：输出section的名字

   ADDRESS：输出section的地址，用于设置VMA。

   OUTPUT-SECTION-COMMAND为以下四种之一:

   - .符号赋值语句

   - .输入section描述

     ```
     FILENAME([EXCLUDE_FILE (FILENAME1 FILENAME2 ...) SECTION1 SECTION2 ...)
     ```

   - .直接包含的数据值

   - .一些特殊的输出section关键字

4. section覆盖描述

#### 赋值语句

#### 表达式

#### 隐式的链接脚本

- 如果指定的输入文件不能识别为object或archive，将会尝试把它作为链接脚本。如果此文件不能按链接脚本解析，链接器将会报错。
- 隐式链接脚本不会取代默认链接脚本。
- 一般来说，隐式的链接脚本仅包含符号赋值，或INPUT,GROUP，或VERSION命令。
- 作为隐式链接脚本的输入文件，其内容会放到读取它的命令的那个位置。这会影响archive搜索。