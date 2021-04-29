#### 简介

共享可变容器。

#### 结构体

##### Ref

##### RefCell

一个动态检查借用规则的可变内存位置。

```rust
pub struct RefCell<T: ?Sized> { /* fields omitted */ }
```

- 实现

  ```rust
  impl<T> RefCell<T>
  
  impl<T: ?Sized> RefCell<T>
    pub fn borrow(&self) -> Ref<'_, T>	// 对封装的值进行不可变借用
  
  impl<T: Default> RefCell<T>
  ```

  

- 特性实现

- 自动特性实现

- 空白实现

