#### 简介

格式化字符串或打印字符串的工具。

#### 宏

Debug

#### 结构体

#### 枚举类型

Alignment

#### 特性

Binary

##### Debug

`?`格式。即把输出格式化为面向程序员的，要调试的内容。若使用可选格式`#?`，输出会更好看些。

```rust
pub trait Debug {
    fn fmt(&self, f: &mut Formatter<'_>) -> Result;
}
```



#### 函数

write

#### 类型定义

Result