#### 简介

对于那些不能“隐式复制”的类型，提供它们所需要的`Clone`特性。

#### 宏

- Clone - 此宏用以生成特性`Clone`的impl。

  ```rust
  macro_rules! Clone {
      ($item:item) => {...};
  }
  ```

#### 特性

- Clone - 一个通用的特性，用于显式地复制对象。

  ```rust
  pub trait Clone {
      pub fn clone(&self) -> self;
      pub fn clone_from(&mut self, source: &self) {...}
  }
  ```

  