##### flatc

```bash
# 编译flatc
git clone https://github.com/google/flatbuffers.git
export CC=/usr/bin/clang
export CXX=/usr/bin/clang++
cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -B build
cd build && make

# 用法
flatc [options] <files> [-- files]
  --cpp, -c # 生成C++头文件
  --python, -p	# 生成Python文件
  --gen-mutable	# 生成可以改变缓冲区的访问器。(缓冲区默认是不可改变的)
```

##### schema语言

`namespace <NameSpace>;`定义命名空间NameSpace。

`enum <name>:<type> {}`定义枚举类型。

`union <name> {}`定义联合类型。

`struct <name> {}`定义结构体。

`table <name> {}`定义列表。列表里的字段不可删除，但可以使用`deprecated`来阻止生成代码访问这个字段。

`root_type`用来声明序列化数据的根列表。

详见如下文档：

- [writing a schema](https://google.github.io/flatbuffers/flatbuffers_guide_writing_schema.html)

- [grammar of the schema language](https://google.github.io/flatbuffers/flatbuffers_grammar.html)

##### 用C++读写FlatBuffers

```cpp
// #include生成的文件并using namespace
// 使用FlatBufferBuilder实例化一个缓冲区
// 在序列化一个对象前，先要序列化其中包含的任何对象
// 最后，调用finish方法结束序列化。
// 保存缓冲区，注意要保存为二进制格式。
// 使用g++或clang++编译器

/* 把buffer写入文件示例代码 */
  std::ofstream outfile;
  outfile.open("csample.bat", std::ios::binary | std::ios::out);
  outfile.write((const char *)builder.GetBufferPointer(),     builder.GetSize());
  outfile.close();

/* 从文件读入buffer示例代码 */
  std::ifstream infile;
  infile.open("csample.bat", std::ios::binary | std::ios::in);
  infile.seekg(0, std::ios::end);
  int length = infile.tellg();
  infile.seekg(0, std::ios::beg);
  char *data = new char[length];
  infile.read(data, length);
  infile.close();
```

详见如下文档：

- [tutorial](https://google.github.io/flatbuffers/flatbuffers_guide_tutorial.html)
- [use cpp](https://google.github.io/flatbuffers/flatbuffers_guide_use_cpp.html)
- [api](https://google.github.io/flatbuffers/group__flatbuffers__cpp__api.html)

##### 用Python读写FlatBuffers

```python
# 把buffer写入文件示例代码
  file = open('psample.bat','wb');
  file.write(builder.Output());
  file.close;
    
# 从文件读入buffer示例代码
  file = open('psample.bat','rb');
  buf = file.read();
  buf = bytearray(buf);
  file.close;
```

详见如下文档：

- [use python](https://google.github.io/flatbuffers/flatbuffers_guide_use_python.html)

