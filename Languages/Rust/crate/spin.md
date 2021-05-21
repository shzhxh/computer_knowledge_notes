#### 简介

提供`std::sync`和`std::lazy`里关于自旋的原语。适用于`no_std`的环境。

#### 重新导出

```rust
pub use mutex::MutexGuard;
```

#### 模块

##### mutex

mutex的锁

```rust
// 模块
spin
ticket

// 结构体
Mutex	// 一个基于自旋的锁，提供对数据的互斥访问
MutexGuard	// 对数据访问提供通用的保护，使用ticket锁或自旋锁
```



#### 类型定义