1. `sudo apt update    //更新软件包到最新状态`

2. `sudo apt install python2.7   //安装python2.7`

3. `sudo apt install nodejs      //安装nodejs`

4. `sudo apt install build-essential 安装gcc`
   `sudo apt install cmake       //安装cmake`

5. `sudo apt install git-core    //安装git-core`

6. `sudo apt install default-jre //安装JavaScript`

7. 编译安装Fastcomp(LLVM + Clang)

  ```
  mkdir myfastcomp
  cd myfastcomp
  git clone https://github.com/kripken/emscripten-fastcomp
  cd emscripten-fastcomp
  git clone https://github.com/kripken/emscripten-fastcomp-clang tools/clang
  mkdir duild
  cd build
  cmake .. -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86;JSBackend" -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -DCLANG_INCLUDE_EXAMPLES=OFF -DCLANG_INCLUDE_TESTS=OFF   // 使用cmake配置（cmake或configure二选一）
  ../configure --enable-optimized --disable-assertions --enable-targets=host,js   // 使用configure配置（cmake或configure二选一）
  cat /proc/cpuinfo | grep "^cpu cores" | uniq    // 查询CPU核心数量，充分利用多核可以加快编译速度
  make -j2    // 上步查到是2核，所以我用-j2参数，慢慢等待吧。。。
  ```

8. `git clone https://github.com/kripken/emscripten.git    //复制Emscripten`

9. 修改配置文件~/.emscripten，将LLVM_ROOT设置为第7步的目录`.../myfastcomp/emscripten-fastcomp/build/bin`

10. `./emcc -v   //在emscripten目录运行此命令，如果没报错即为安装正确`

11. `./emcc tests/hello_world.c   //如果没报错即为运行正确`
