#### 简介

slice管理和操作。详见`std::slice`

#### 函数

##### from_raw_parts

```rust
// 从指针和长度生成一个slice
// len : 元素数量(并不是指字节数)
pub unsafe fn from_raw_parts<'a,T>(data:*const T,len:usize) -> &'a[T]
```



