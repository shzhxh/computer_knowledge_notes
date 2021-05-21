#### 简介

包含一个UTF-8编码的文件，把它作为字符串来处理。文件的内容将会以`&'static str`的形式存在。

```rust
macro_rules! include_str {
    ($file:expr $(,)?) => {...};
}
```

