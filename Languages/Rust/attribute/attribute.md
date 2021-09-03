#### 属性

属性是一个通用的，格式自由的metadatum。(metadatum: one piece of metadate; a piece of information describing a piece of data.)

语法：

```rust
#![Attr]	// 内部属性，作用于声明[Attr]的地方
#[Attr]		// 外部属性，作用于[Attr]之后
```

分类：

- 内建属性
- 宏属性
- 派生宏辅助属性
- 工具属性

可应用于：

##### 元条目属性语法

##### 活跃的或内部的

##### 工具属性

##### 内建属性

```rust
// 条件编译
cfg	// 控制条件编译
cfg_attr	// 有条件地包含属性
// 测试
// 派生
derive		// 自动特性实现(derive的意思是获取，故在此处的意思是自动获取某个特性)
// 宏
macro_use	// 扩展宏的可见性。或从其它crate导入宏(后面要跟extern crate声明)。
// 诊断
allow, warn, deny, forbid	// 改变默认的检测级别
    allow(c)	// 撤销对c的检查，这样违规行为将不会被报告
	warn(c)		// 对c的违规行为将发出警告，但仍然会继续编译
    deny(c)		// 如遇到c的违规行为，将发出error信号
    forbid(c)	// 类似于deny(c)，且禁止更改检测级别
// ABI，链接，符号，FFI
no_main	// 不会加载"main"标记(当使用了no_std属性，rust就不会走正常的main函数流程，这时就需要使用no_main属性,详见参考文档的"rust-no-main")
no_mangle	// 禁止符号名编码(这样编译器就不会mangle函数名，从而使其它语言可以调用Rust函数的接口)
// 代码生成
// 文档
// 预加载
no_std	// 不会预加载std
// 模块
// 限制
// 运行时
global_allocator	// 实现了GlobalAlloc特性的静态条目用此属性来设置全局内存分配器.
// 性质
feature	// 使能开发中的，或实验性的编译器属性。
// 类型系统
```

#### 参考文档

[rust-no-main](https://12101111.github.io/rust-no-main/)
