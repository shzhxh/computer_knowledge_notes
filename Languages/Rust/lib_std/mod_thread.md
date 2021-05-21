#### 简介

管理本地线程。一个正在运行的程序是由一些本地OS线程组成的，每个线程都有自己的栈和状态。

```rust
/* 创建线程 */
thread::spawn(move || {});

/* 配置线程 */
thread::Builder::new().name("child1".to_string()).spawn(move || {});

/* Thread类型 */
// 使用Thread类型来代表线程，有两种方法得到Thread类型：
thread::spawn	// 方法一：创建线程
thread::current	// 方法二：请求当前线程

/* Thread-local存储 */

/* 给线程取名 */
Builder::name	// 使用Builder构建线程，然后给它命名
Thread::name	// 从线程中检索线程名

/* 栈的大小 */
```

#### 结构体

#### 函数

- sleep - 让当前线程休眠。休眠时间可能比dur更长，但绝不会更短。

  ```rust
  pub fn sleep(dur: Duration)
  ```

  

#### 类型定义