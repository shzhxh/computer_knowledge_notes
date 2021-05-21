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

##### use

引入来自于crate或模块的条目。

可以使用像`crate`、`super`、`self`这样的限定符。注意，当使用通配符`*`来导入类型的时候，它**不会**导入这个类型的方法。

```rust
use a::b::{c,d,e::f,g::h::i};	// 使用大括号导入若干路径，这些路径有共同的前缀"a::b"
use a::b::{self,c,d::e};		// 使用self关键字导入若干路径的公共父模块"a::b"
use a::b::{self as ab, c as abc};	// 使用as关键字给导入的对象取个本地的名字
use a::b::*;					// 使用通配符 * 号来匹配所有路径
use a::b::{self,c,d::{*,e::f}};	// 多层嵌套
pub use a::b;					// 使用pub关键字重新导出
use std::io::Read as _;		// 使用 _ 号只导入特性的方法而不绑定它的名字(避免名字冲突)
```

