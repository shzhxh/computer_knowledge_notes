#### 简介

时间量化。

#### 结体体

- Duration - 表示时间的跨度，通过用于系统超时。

  ```rust
  pub struct Duration { /* fields omitted */}
  
  /* 实现 */
  pub const fn from_secs(secv: u64) -> Duration
  	// 从秒数来创建Duration
  
  /* 实现的特性 */
  /* 自动实现的特性 */
  /* 实现的空白特性 */
  ```

  

#### 常量

- UNIX_EPOCH