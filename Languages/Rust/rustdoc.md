#### 3 怎么写文档

好的文档不是天生的。编写好的文档是困难的。它即需要专家知识，也需要新手的视角。因此，一般的文档常常忽略实现细节，或者给读者留下未回答的问题。


Rust文档有一些原则，有利于指导人们完成文档化的过程，以便每个人都有充分的机会使用代码。


本章不仅介绍了如何编写文档，而且特别介绍了如何编写**好**的文档。尽可能清晰和完整是非常重要的。作为经验法则:你为你的库写的文档越多越好。如果一个条目是公共的，那就应该为它写个文档。

##### 初尝试

为库写文档应该从首页(front-page)文档开始。以`hashbrown`库为例，它在库级的文档总结了库的角色，提供了解释技术细节的链接，并解释了为什么要使用这个库。


在介绍了库之后，重要的是在首页(front-page)给出一个如何在现实世界中使用库的例子。在示例中坚持库的角色且不要使用快捷方式，这样可以让那些可能通过复制和粘贴示例开始学习的用户受益。

`futures`使用内联注释逐行解释使用`Future`的复杂性，因为一个人第一次接触rust的`Future`可能就是这个例子。

`backtrace`文档贯穿整个过程，解释对`Cargo.tom`文件所做的更改，将命令行参数传递给编译器，并展示了一个反向跟踪的简明示例。


最后，首页最终可以成为一个如何使用库的全面的参考，比如像`regex`那样。在这个首页中，概述了所有的需求，展示了边缘情况，并提供了实际的例子。首页继续展示如何使用正则表达式，并在最后展示库的特性。


不要担心将刚开始使用的自己的库与其他更完善的库进行比较。为了使文档更加完善，可以循序渐进地开始，并加入介绍、示例和特性。罗马不是一天建成的!

`lib.rs`中的前几行将组成首页，他们使用不同于其他rust文档的格式。它们应该以`//!`开头，这代表了模块级或库级文档。下面是一个简单的例子:

```rust
//! Fast and easy queue abstraction.
//!
//! Provides an abstraction over a queue.  When the abstraction is used
//! there are these advantages:
//! - Fast
//! - [`Easy`]
//!
//! [`Easy`]: http://thatwaseasy.example.com

/// This module makes it easy.
pub mod easy {

    /// Use the abstraction function to do this specific thing.
    pub fn abstraction() {}

}
```

理想情况下，文档第一行应该没有深度技术细节，它应该很好的描述这个库在rust生态系统中适合的位置。用户应该在阅读这一行之后就应该知道这个库是否满足他们的案例。

##### 组件文档化

无论是模块、结构体、函数还是宏:所有公共API都应该有文档。没有人会抱怨文档太多!

建议每一条目的文档遵循以下基本结构:

```
[简短的语句解释它是什么]

[更详细的解释]

[至少有一个可以粘贴\复制的代码，使得用户可以尝试它]

[如必要的话更进一步的解释]
```

如上描述的基本结构应该简单易懂；虽然您可能认为代码示例微不足道，但示例非常重要，因为它们可以帮助用户理解项目是什么、如何使用它以及它存在的目的。

让我们看一个来自标准库的例子，看一下`std::env::args()`函数:

````
Returns the arguments which this program was started with (normally passed
via the command line).

The first element is traditionally the path of the executable, but it can be
set to arbitrary text, and may not even exist. This means this property should
not be relied upon for security purposes.

On Unix systems shell usually expands unquoted arguments with glob patterns
(such as `*` and `?`). On Windows this is not done, and such arguments are
passed as-is.

# Panics

The returned iterator will panic during iteration if any argument to the
process is not valid unicode. If this is not desired,
use the [`args_os`] function instead.

# Examples

```
use std::env;

// Prints each argument on a separate line
for argument in env::args() {
    println!("{}", argument);
}
```

[`args_os`]: ./fn.args_os.html
````

第一个空行之前的所有内容将被重用，以在搜索和模块概览中描述组件。例如，上面的函数`std::env::args()`将在`std::env`模块文档中显示。将摘要保持在一行是一个很好的实践：简洁的写作是一个好文档的标志。


因为类型系统在定义函数传递和返回的类型方面做得很好，所以显式地将其写入文档没有任何好处，尤其是`rustdoc`还为函数签名中的所有类型添加了超链接。


在上面的例子中，“Panics”段解释了代码可能突然退出的时间，这可以帮助读者避免陷入panic。如果知道代码中的边缘情况，建议每次都加入"Panic"段。


如您所见，它遵循了上面详述的结构:它以一个简短的句子开始解释函数的功能，然后提供更多的信息，最后提供一个代码示例。

##### Markdown

`rustdoc`使用的是[commonMark Markdown格式](https://commonmark.org/)。

除了commonMark语法，`rustdoc`还支持：

- 删除线

  `~~删除线~~`：~~删除线~~

- 脚注

  ```
  一个脚注[^note]的示例.
  
  [^note]: 脚注的内容，会在底部渲染。
  ```

  一个脚注[^note]的示例.

  [^note]: 脚注的内容，会在底部渲染。

- 表格

- 任务列表

- 自动标点转换

  