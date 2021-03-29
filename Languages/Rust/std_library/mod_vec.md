#### 简介

一个可增长的数组类型，此数组类型的实例通过堆来分配。

#### 结构体

- Drain - 

- Intolter - 

- Splice - 

- Vec - 一个可增长的数组类型。写法为`Vec<T>`。

  ```rust
  pub struct Vec<T, A = Global>
  where
      A: Allocator,
      { /* fields omitted */ }
  
  /* 方法 */
  pub fn len(&self) -> usize	// 返回此vector里元素的个数，即它的“长度”。
  ```

