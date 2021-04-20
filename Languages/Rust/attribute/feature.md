详见[the unstable book](https://doc.rust-lang.org/stable/unstable-book/index.html)

#### 编译器标志

#### 语言属性

#### 库属性

##### global_asm

`global_asm!`宏使得程序员可以在函数体之外写汇编，通过rustc和llvm传递给汇编器。这个宏是[llvm模块级内联汇编](http://llvm.org/docs/LangRef.html#module-level-inline-assembly)的简单接口。

如果不需要`global_asm!`的强大和灵活，且仅在函数体内使用内联汇编，应使用`asm`属性。

##### llvm_asm

`llvm_asm!`宏支持内联汇编，格式如下：

```rust
llvm_asm!(assembly template
    : output operands
    : input operands
    : clobbers
    : options
    );
```

详细的格式请参考[LLVM's inline assembler expressions](http://llvm.org/docs/LangRef.html#inline-assembler-expressions)

相比于`global_asm`，它能力会弱一点，但有更精细的控制。

##### panic_info_message