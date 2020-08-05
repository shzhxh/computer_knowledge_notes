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

链接器默认允许分配所有可用内存，可用`MEMORY`命令来覆盖此默认配置。

`MEMORY`命令描述了目标内存区域的位置和大小。它还描述了链接器对此内存区域的权限。

```
MEMORY {
NAME1 [(ATTR)] : ORIGIN = ORIGIN1, LENGTH = LEN1
NAME2 [(ATTR)] : ORIGIN = ORIGIN2, LENGTH = LEN2
…
}
```

NAME:内存区域的名字。仅用于链接脚本内对此内存区域的引用，即在链接脚本之外此名称是无意义。

ATTR:该内存区域的属性。R只读，W读写，X可执行，A可分配，I已初始化，L同I，!反转它后面跟着的任意属性的意思。

ORIGIN:此内存区域的开始地址，可简写成org或o.

LENGTH:此内存区域的大小，默认以bytes为单位，也可显式地指定K(kilobytes), M(megabytes)等单位，可简写成len或l.

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
   SECTION-NAME [ADDRESS] [(TYPE)] : 
     [AT(LMA)]
     [ALIGN(section_align) | ALIGN_WITH_INPUT]
     [SUBALIGN(subsection_align)]
     [constraint]
     {
       OUTPUT-SECTION-COMMAND
       OUTPUT-SECTION-COMMAND
       …
     } [>REGION] [AT>LMA_REGION] [:PHDR HDR ...] [=FILLEXP]
   ```

   SECTION-NAME：输出section的名字

   ADDRESS：输出section的地址，用于设置VMA。

   OUTPUT-SECTION-COMMAND为以下四种之一:

   - .符号赋值语句，详见赋值语句

   - .输入section描述(它是最基本的操作，在OUTPUT-SECTION-COMMAND中最为常见)

     用于把输入文件里指定的section包含到输出文件的section里，如果不包含某些输入文件则要使用EXCLUDE_FILE。

     ```
     FILENAME([EXCLUDE_FILE (FILENAME1 FILENAME2 ...) SECTION1 SECTION2 ...)
     ```

     输入section描述由文件名跟着圆括号内的section名组成。文件名和section名可以使用通配符模型。

     EXCLUDE_FILE：用于指定不用来匹配的文件列表。

   - .直接包含的数据值

   - .特殊的输出section关键字

4. section覆盖描述

#### 赋值语句

#### 表达式

#### 隐式的链接脚本

- 如果指定的输入文件不能识别为object或archive，将会尝试把它作为链接脚本。如果此文件不能按链接脚本解析，链接器将会报错。
- 隐式链接脚本不会取代默认链接脚本。
- 一般来说，隐式的链接脚本仅包含符号赋值，或INPUT,GROUP，或VERSION命令。
- 作为隐式链接脚本的输入文件，其内容会放到读取它的命令的那个位置。这会影响archive搜索。

#### 参考资料

[Linker Scripts](https://sourceware.org/binutils/docs/ld/Scripts.html)