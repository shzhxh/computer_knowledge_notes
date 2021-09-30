简单的usb host driver

#### 结构体

```rust
// Boot protocol keyboard driver
pub struct BootKeyboard<F> {
    devices: [Option<Device>; MAX_DEVICES],
    callback: F,
}

// 关联函数new，用于创建驱动的实例
pub fn new(callback: F) -> Self
```

