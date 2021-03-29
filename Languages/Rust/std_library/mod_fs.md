#### 简介

对文件系统进行管理操作。本模块中的所有操作都是跨平台的。

#### 结构体

#### 函数

- read_to_string - 把文件的所有内容都读到一个字符串里。

  ```rust
  pub fn read_to_string<P: AsRef<Path>>(path: P) -> Result<String>
  ```

  