#### 简介

使用`Result`类型进行错误处理。

#### 结构体

#### 枚举

- Result - 一种类型，代表成功`Ok`或失败`Err`。

  ```rust
  pub enum Result<T, E> {
      Ok(T),
      Err(E),
  }
  ```

  