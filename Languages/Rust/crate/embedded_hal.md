#### 简介

适用于嵌入式系统的硬件抽象层。

#### 模块

- serial - 串行接口

  ```rust
  /*
   * 特性
   */
  // Read - 串行接口的读取
      type Error	// Read error
      fn try_read(&mut self) -> Result<Word, Self:Error>
  // Write - 串行接口的写入
      type Error	// Write error
      fn try_write(&mut self, word: Word) -> Result<(), Self::Error>
          // 写一个word到串行接口
      fn try_flush(&mut self) -> Result<(), Self::Error>
          // 确保之前的写操作都已完成
  ```

  

