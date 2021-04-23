#### 简介

实现了一些特性，用以重载操作符。

#### 结构体

- Range - 取值范围是左闭右开型的。

  ```rust
  pub struct Range<Idx> {
      pub start: Idx;	// 范围的下界(包含)
      pub end: Idx;	// 范围的上界(不包含)
  }
  ```

  实现

  特性的实现

  ```rust
  impl<A> Iterator for Range<A>
  ```

  

  自动特性的实现

  空白实现

- RangeFrom

- RangeFull

- RangeInclusive

- RangeTo

- RangeToInclusive

#### 数组

#### 特性

- Fn - The version of the call operator that takes an immutable receiver.

  ```rust
  pub trait Fn<Args>: FnMut<Args> {
      pub extern "rust-call" fn call(&self, args: Args) -> Self::Output;
  }
  ```

  