### 来源：
* [知乎](https://www.zhihu.com/question/25483589)  
* [阮一峰_asm.js和Emscripten入门教程](http://www.ruanyifeng.com/blog/2017/09/asmjs_emscripten.html)  
* 《JavaScript高级程序设计》

### 工具
* asm.js  
  是由Mozilla提出的一个基于JS的语法标准，主要是为了解决JS引擎的执行效率问题
* Emscripten  
  是Mozilla的一个实验性项目，目的是把C/C++开发的应用编译成JS或HTML5的应用  
  emcc是Emscripten的编译器前端  
  **注意：** 在ubuntu系统通过apt命令安装存在问题，应选择从源码编译安装
* uglifyjs是用JavaScript写的压缩JavaScript代码的工具  
  **格式**：`uglifyjs [选项] [文件名]`   
  **--compress**：启用压缩选项  
  **--mangle**：输出变量名替换后的文件

### JavaScript语法
* JavaScript = ECMAscript + DOM + BOM
* 数据类型 (5种基本数据类型和2种复杂数据类型)
  1. 基本数据类型：Undefined, Boolean, Number, String, Null
  2. 复杂数据类型：Object, Array
  3. 变量：字母、数字、$、下划线的组合
