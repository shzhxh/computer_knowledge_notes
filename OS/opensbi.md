#### OpenSBI库

OpenSBI提供了两种静态链接库：

1. libsbi.a - 平台无关，实现了SBI规范的接口，由于不包含平台相关的部分，所以必须实现与平台相关的钩子且链接到本库里。
2. libplatsbi.a - 即包括了libsbi.a，也包括了平台相关的钩子。

#### 平台支持

平台支持是通过数据结构`sbi_platform`来实现的。