线程安全的引用计数指针。

#### 结构体

##### Arc

Arc的意思是自动引用计数(Atomically Reference Counted)。这是一个线程安全的引用计数指针。

```rust
pub fn new(data: T) -> Arc<T> {}	// 构造一个新的Arc<T>
```



##### Weak