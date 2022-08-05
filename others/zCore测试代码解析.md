#### 标题

|                      zCore测试代码解析                       |
| :----------------------------------------------------------: |
| 清华大学计算机系工程师<br>负责OS测例的维护与开发<br>本文档地址：[https://github.com/shzhxh/computer_knowledge_notes/blob/master/others/zCore测试代码解析.md]() |

#### 需求分析

1. 我写了一个操作系统，怎么验证实现是正确的？
   - zCore支持Zircon和Linux两套系统调用
   - zCore支持libos和bare两种模式
   - zCore支持X64, AArch64, Riscv64三种架构
   - zCore支持Qemu和多种开发板等硬件环境
2. 多人向操作系统贡献代码，怎么保证代码质量？
   - 语言级别的检查，格式与语法检查
   - 对系统调用的检查，新的提交不能让原先没问题的测例报错

#### 设计分析

1. core-test测试Zircon系统调用，libc-test和oscomp测试Linux系统调用。
   - core-test是对Zircon内核的功能进行测试，详见fuchsia源码的`zircon/system/utest/core`目录。
   - libc-test是对Musl标准库进行的测试，源码在https://github.com/rcore-os/libc-test。
   - oscomp是全国大学生能力大赛里测试系统调用的测例，源码在https://github.com/oscomp/testsuits-for-oskernel/tree/main/riscv-syscalls-testing
   
2. 利用github action进行CI测试，新的提交不能让原先没问题的测例报错

   - 在`.github/workflows/*.yml`使用如下配置可以在推送或提交pr的时候执行指定的操作，详见https://docs.github.com/cn/actions/using-workflows/triggering-a-workflow

     ```yaml
     on:
       push:
       pull_request:
     ```

     

3. 利用Makefile, xtask, cargo实现基本的操作

   - xtask

     `.cargo/config.toml`里指明了`cargo xxx`命令其实对应的`cargo run --package xtask --release --xxx`命令。

     xtask包的源码在`./xtask`目录。

   - `./Makefile`只是对`xtask`命令的直接封装，`./zCore/Makefile`主要用来编译、运行、调试内核，但在顶层的Makefile已提供了相应的功能。

   - cargo

     是对rust包进行管理的命令，xtask或Makefile最终都要调用它。

4. 在独立的仓库里进行自动化测试
   - 系统工程师不能即当运动员，又当裁判员。要搞定问题，而不是搞定测例。
   - 新编写的测例可以放在此仓库。
   - 系统工程师一般不需要主动修改测例的状态。如果一个测例不应该通过，应该设置内核不要让它通过，而不是把测例状态改为不通过。

#### 实现介绍

1. xtask的介绍

   ```rust
   // 使用clap库进行命令行参数的解析
   // main函数是入口，它通过match命令来分发子命令。
   // 子命令rootfs, libc-test, other-test都被分发给arg.linux_rootfs().xxx()
   // linux_rootfs()的作用是指定架构
   // xxx()函数做具体的工作
   //   1. 确保基本镜像目录的存在(下载交叉工具链，编译busybox, 复制动态链接库)
   //   2. 如果是二进制文件直接下载并解压
   //   3. 如果是源文件则把它编译成二进制文件
   // 子命令image被分发给arg.linux_rootfs().image()
   // image()函数做具体的工作
   //   1. 确保镜像目录存在
   //   2. 使用fuse命令从镜像目录生成镜像文件
   ```

   

2. 两个Makefile的对比

   | zCore子目录下的操作                                       | 顶层目录下的操作                                             |
   | --------------------------------------------------------- | ------------------------------------------------------------ |
   | make run LINUX=1 ARCH=riscv64 MODE=release                | cargo qemu --arch riscv64                                    |
   | make run_d1 PLATFORM=d1 ARCH=riscv64 LINUX=1 MODE=release | cargo bin --arch riscv64 --features "linux board-d1 link-user-img" --output zcore_d1.bin |

   

3. 自动化测试的脚本

   1. **utils.load_testcases函数**

      1. core_test的全部测例放在zircon_core_test里，通过架构(目前仅x86_64)和运行模式(libos/bare)分类。每个测例还包含了运行结果的信息(OK/FAILED/TIMEOUT)，这样已通过和未通过的测例就可以在一个文件里表示了。
      2. libc_test的全部测例放在linux_libc_test里，通过架构(x86_64/riscv64)和运行模式(libos/bare)分类。每个测例还包含了运行结果的信息(OK/FAILED/TIMEOUT)，这样已通过和未通过的测例就可以在一个文件里表示了。
      3. 通过`load_testcases()`提取测例，它定义在`test.py`。工作过程：就是从测例文件中提取出二元组(testcase, TestStatus)。

   2. **utils.TestRunner类**

      ```python
      # 创建对象self的时候会创建对象self.logger
      def __init__(self)
      # 销毁对象的时候执行teardown()方法。但实际上不做任何事情。
      def __del__(self)		
      # 生成命令行要执行的构建命令。(注：需要重载)
      def build_cmdline(self)
      # 生成命令行要执行的测例命令。(注：需要重载)
      def run_cmdline(self)
      # 执行构建命令。最终通过subprocess.run()实现。
      def build(self)
      # 遍历testcases，依次执行测例
      def run_all(self, testcases, fast, timeout) -> bool
      ```

   3. **log.Logger类**

      ```python
      # 创建对象self的时候打开文件log_file_name
      def __init__(self, log_file_name)
      # 销毁对象的时候关闭文件
      def __del__(self)
      # 把message输出到标准输出和文件
      def print(self, message)
      # self.print(message+"\n")
      def println(self, message)
      ```

   4. **core test执行流程**

      1. 解析参数。

         - -l，则args.libos为True。
         - -f，则args.fast为True。
         - -t，则args.test为True。

      2. 全局变量赋值

      3. 创建ZirconTestRunner对象, 继承自TestRunner

         ```python
         BASE_CMD = "cd ./zCore && make MODE=release ZBI=core-tests TEST=1"
         # 如定义args.libos则返回self.BASE_CMD + "LIBOS=1"
         # 如未定义args.libos则返回self.BASE_CMD
         def build_cmdline(self) -> str
         # 如定义args.libos则返回"../target/release/zcore $(ZBI_PATH) $(CMDLINE_BASE)+name"
         # 如未定义args.libos则返回self.BASE_CMD+"CMDLINE=$(CMDLINE_BASE)+name"+"justrun"
         def run_cmdline(self, name) -> str
         ```

         

      4. 执行runner.build构建内核

         ```bash
         cd zCore
         make MODE=release ZBI=core-tests TEST=1 LIBOS=1	# if args.libos
         make MODE=release ZBI=core-tests TEST=1	# if not args.libos
         ```

         

      5. 如定义了args.test，则执行runner.run_one()只执行一个测例。

         ```bash
         # if args.libos
         ../target/release/zcore ../prebuilt/zircon/x64/core-tests.zbi LOG=error:userboot=test/core-standalone-test:userboot.shutdown:core-tests=<testcase>
         # if not args.libos
         make MODE=release ZBI=core-tests TEST=1 CMDLINE='LOG=error:userboot=test/core-standalone-test:userboot.shutdown:core-tests=<testcase>' justrun
         ```

         

      6. 如没有定义args.test，则执行runner.run_all()。

         ```bash
         runner.set_logger()	# 打开日志文件zircon_core_test_<arch>_<mode>.log，这样就可以将信息写入日志了
         load_testcases()	# 从testcases/zircon_core_test/<arch>_<mode>.txt文件中读取测例
         
         # 接下来是runner.runall()
         # 1. 在for循环中遍历所有测例，通过self.run_one()执行每个测例
         # 2. for循环执行完毕，执行print_result()打印结果
         ```

   5. **libc test执行流程**
      
      1. 解析参数
      
      2. 全局变量赋值
      
      3. 创建LinuxTestRunner对象
      
      4. 执行runner.build构建内核
      
         ```bash
         # if args.libos
         cd ../zCore && make MODE=release LINUX=1 TEST=1 ARCH=<arch> LIBOS=1
         # if not args.libos
         cd ../zCore && make MODE=release LINUX=1 TEST=1 ARCH=<arch>
         ```
      
      5. 如定义了args.test则只运行一个测例
      
         ```bash
         # if args.libos
         cd .. && LOG=warn ./target/release/zcore <testcase>
         
         # if not args.libos
         cd ../zCore 
         make MODE=release LINUX=1 TEST=1 ARCH=<arch> CMDLINE='LOG=warn:ROOTPROC=<testcase>' justrun
         ```
      
         
      
      6. 如没有定义args.test则运行常规测试或全部测试
      
         ```bash
         runner.setlog()	# 打开日志文件linux_libc_test_<arch>_<mode>.log，用来记录日志
         load_testcases()	# 把所有测例都保存在一个列表里
         
         # runner.runall
         # 1. 在for循环中遍历所有测例，通过self.run_one()执行每个测例
         # 2. for循环执行完毕，执行print_result()打印结果
         ```
      
         

4. github action脚本

   1. 一个仓库的action包含一个以上的workflow，一个workflow表现为一个yml文件，每个workflow都有各自的触发条件。
   2. 一个workflow包含一个以上的job,job之间是并行的，job内部是串行的。一个workflow就是具有共同目标的job的集合。
   3. 一个job包含若干step,一个step包含若干action.action就是组成step的命令。
   4. github action里可以缓存文件，这将大大地降低测试时间。


#### 思考不足

1. xtask在报错后定位问题困难

   可能是系统里缺少某个命令

   可能是依赖的文件或目录没有下载好

2. 标准库的运行失败了，但测例通过了

   libc-test是为测试标准库而服务的，它假定系统调用是没问题的。然而测系统调用的时候，应该假定系统调用是有问题的。

#### 改进思路

不是测标准库，而是测系统调用。

oscomp的设计思想是做一层系统调用的接口，基于此接口编译应用程序就可以测试指定的系统调用。

oscomp的问题是只能测一种架构。oscomp里测例可以进一步发展，基于标准库，但严格地控制只使用最低层的系统调用及系统调用无关的函数。应该可以最大限度地测系统调用，且一套代码测多个平台。