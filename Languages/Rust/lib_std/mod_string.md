#### 简介

UTF-8编码的，可增长的字符串。

#### 结构体

- String - 一种UTF-8编码的，可增长的字符串。它是最常用的字符串类型，对字符串的内容有所有权，它其实是对`str`类型的借用。

  ```rust
  pub struct String { /* fields omitted */ }
  
/* 示例 */
  
  /* UTF-8 */
  
  /* Deref */
  // String实现了Deref特性，因而继承了str的所有方法。并且，这还意味着可以用&str的方式向函数传递String。
  
  /* 表示 */
  // String由三个部分组成：指向字符串的指针，当前字符串的长度，容量。String的缓冲区总是保存在堆上。
  
  /* 实现 */
  
  /* Deref的方法 */
  
  /* 实现的特性 */
  From	// from方法即来自于此特性
  
  /* 自动实现的特性 */
  
  /* 空白的实现 */
  ```
  
  

#### 特性

- ToString - 用于把一个值转换成`String`。

#### 类型定义

- ParseError - `Infallible`的别名。