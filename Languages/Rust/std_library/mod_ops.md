#### 简介

实现了一些特性，用以重载操作符。

#### 结构体

#### 数组

#### 特性

- Fn - The version of the call operator that takes an immutable receiver.

  ```rust
  pub trait Fn<Args>: FnMut<Args> {
      pub extern "rust-call" fn call(&self, args: Args) -> Self::Output;
  }
  ```

  