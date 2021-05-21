#### 简介

使用了buddy系统的裸金属分配器。

#### 模块

##### linked_list

#### 结构体

##### FrameAllocator

##### Heap

次序可配置的堆

##### LockedFrameAllocator

##### LockedHeap

堆的带锁版本

```rust
// 实现
pub const fn new() -> Self	// 创建一个空堆
pub const fn empty9) -> Self	// 创建一个堆

// 来自于Deref<Target = Mutex<Heap<ORDER>>>的方法
pub fn is_locked(&self) -> bool		// 如果持有锁则返回true
pub fn lock(&self) -> MutexGuard<'_, T>	// 锁定Mutex并返回guard，使用此guard可以访问内部数据
pub unsafe fn force_unlock(&self)	// 强制解锁Mutex
pub fn try_lock(&self) -> Option<MutexGuard<'_, T>>	// 尝试锁定Mutex，如成功则返回一个guard
```



##### LockedHeapWithRescue