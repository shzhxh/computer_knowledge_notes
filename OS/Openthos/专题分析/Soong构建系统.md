在Android7.0之前，使用的是GNU Make。从Android7.0开始，引入Soong。

#### Android.bp文件的格式

Android.bp的设计原则是简约，它不包含条件或控制流语句，所有的复杂性都是由go语言写的内在逻辑来控制的。

##### 模块Modules

模块类型详见：[module type](https://ci.android.com/builds/submitted/6066704/linux/latest/view/soong_build.html)。模块是由模块类型+一组键值对组成的。格式如下：

```
cc_binary {
    name: "gzip",
    srcs: ["src/test/minigzip.c"],
    shared_libs: ["libz"],
    stl: "none",
}
```

- 模块名

  cc_binary用于生成可以在设备上跑的二进制。

  filegroup包含一个文件的列表，用于其它模块里属性的引用(使用`:<name>`语法)，或者在包之间导出文件。

- 属性

  `name`属性是必须的，它表示模块的名称。其值在所有模块里必须是唯一的。

  `srcs`，表示用来编译C/C++模块的源文件列表。也可以使用语法`:module`来引用其它模块的输出。

  `shared_libs`，表示其它模块的列表，这些模块将被动态链接到本模块中。

  `stl`，选项要使用的STL库。可能的值是"libc++", "libc++_static", "libstdc++"或"none"。

##### 类型Types

变量和属性是强制类型的，变量的类型由第一次赋值时自动设定，属性的类型由模块类型静态设定。支持的类型有：

- 布尔型(true, false)
- 整型(int)
- 字符串(string)
- 字符串列表(["string1","string2"])
- Maps ({key1: "value1", key2: ["value2"]})

Maps可以包含任何类型的值，包括嵌套maps。

##### Globs

Glob模式是包含通配符的匹配模式。`*`可以匹配多个字符，`**`可以匹配多个路径。

##### 变量Variables

变量的作用域为文件的剩余部分，以及子bp文件里。变量是不可变的，但有一个例外：可以使用`+=`来追加一个值，但仅限于它们被引用前。

##### 注释Comments

支持`/* */`和`//`。

##### 操作符Operators

可以使用`+`操作符追加字符串，字符串列表或maps。也可以用`+`给整数求和。两个map执行`+`操作将使它们各自拥有对方的键值。

##### 条件判断Conditionals

大多数情况下，条件判断都可以写成map里的一个属性，由顶层的属性来决定使用哪一个的值。

例如，为了支持不同架构：

```
cc_library {
    ...
    srcs: ["generic.cpp"],
    arch: {
        arm: {
            srcs: ["arm.cpp"],
        },
        x86: {
            srcs: ["x86.cpp"],
        },
    },
}

```



##### 规范化Formatter

```
bpfmt -w .	# 递归地重新格式化当前目录下的所有的Android.bp文件
```

规范的格式包括：

- 四个空格的缩进
- 多元素列表中每个元素后的新行
- 列表和maps里的尾随逗号

#### 参考文档

- [Soong build system](https://source.android.google.cn/setup/build)