#### 概述

这个模块用来检查和操作进程的环境。包括环境变量、进程参数、当前目录和其它的各种重要目录。

#### 模块

- consts - 与当前目标相关的常数

#### 结构体

- Args - 进程参数的迭代器，为每个参数都生成一个`String`类型的值。

  ```rust
  pub struct Args { /* fields omitted */ }
  ```

- ArgsOs

- JoinPathsError

- SplitPaths

- Vars

- VarsOs

#### 枚举类型

- VarError

#### 函数

- args - 返回传递给本程序的命令行参数。

  ```rust
  pub fn args() -> Args
  ```

- args_os

- current_dir

- current_exe

- home_dir

- join_paths

- remove_var

- set_current_dir

- set_var

- split_paths

- temp_dir

- var

- var_os

- vars

- vars_os