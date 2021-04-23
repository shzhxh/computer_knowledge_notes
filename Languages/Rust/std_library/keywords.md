##### as

类型之间的强制转换，或给导入的模块重命名。

```rust
// 类型之间的强制转换
let thing1: u8 = 89.0 as u8;	// 89.0被转换为u8类型，即89
assert_eq!('B' as u32, 66);		// 'B'被转换为u32类型
assert_eq!(thing1 as char, 'Y')	// thing1被转换为char类型
let thing2: f32 = thing1 as f32 +10.5;	// thing1被转换为f32类型
assert_eq!(true as u8 + thing2 as u8, 100);	// true和thing2被转换为u8类型
	// true会被转换为1，而false会被转换为0

// 给用use或“extern crate”语句导入的模块重命名
use std::{mem as memory, net as network};	// memory指代mem，network指代net
```

