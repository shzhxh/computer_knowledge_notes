#### 简介

内存分配APIs。

#### 结构体

##### AllocError

##### Global

##### Layout

##### LayoutError

#### 特性

##### Allocator

##### GlobalAlloc

这是一个内存分配器，可通过`#[global_allocator]`属性注册为标准库的默认值。

```rust
pub unsafe trait GlobalAlloc {
    pub unsafe fn alloc(&self, layout: Layout) -> *mut u8;
    pub unsafe fn dealloc(&self, ptr: *mut u8, layout: Layout);

    pub unsafe fn alloc_zeroed(&self, layout: Layout) -> *mut u8 { ... }
    pub unsafe fn realloc(
        &self, 
        ptr: *mut u8, 
        layout: Layout, 
        new_size: usize
    ) -> *mut u8 { ... }
}
```



#### 函数

##### alloc

使用全局分配器分配内存

```rust
pub unsafe fn alloc(layout: Layout) -> *mut u8
```



##### alloc_zeroed

##### dealloc

##### handle_alloc_error

##### realloc

#### 类型定义

##### LayoutErr

