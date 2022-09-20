# SFS文件系统

## 概览

| 层级           | 描述                             | 代码                                                     |
| -------------- | -------------------------------- | -------------------------------------------------------- |
| 索引结点层     | 文件控制块数据结构与处理         | `rcore-fs::vfs::FileSystem`                              |
| 磁盘块管理层   | 定义磁盘文件系统的数据结构及处理 | `rcore-fs-sfs::SimpleFileSystem`                         |
| 磁盘数据结构层 | 磁盘上的数据结构及处理           | `rcore-fs-sfs::struct::{SuperBlock,DiskInode,DiskEntry}` |
| 块缓冲层       | 在内存中缓存磁盘数据             | rcore-fs::dev::block_cache::{BlockCache,LRU}             |
| 块设备接口层   | 定义读写磁盘的接口               | `rcore-fs::dev::BlockDevice`                             |

制作sfs镜像

```bash
rcore-fs-fuse sfs.img rootfs zip	# 从目录rootfs生成镜像sfs.img
rcore-fs-fuse dir image git-version	# 版本验证，应为7f5eea
cargo install rcore-fs-fuse --git https://github.com/rcore-os/rcore-fs --rev 7f5eea --force	# 安装版本号为7f5eea的rcore-fs-fuse
```

## 磁盘结构

| 名称           | 位置  | 大小                      |
| -------------- | ----- | ------------------------- |
| 超级块         | 0号块 | 1个块                     |
| 根目录索引结点 | 1号块 | 1个块                     |
| 空闲位图区     | 2号块 | SuperBlock.freemap_blocks |
| 索引结点区     |       |                           |
| 数据区         |       |                           |

块大小：4k字节

inode直接块：12

# rcore-fs

zCore使用的文件系统的库是[rcore-fs](https://github.com/rcore-os/rcore-fs)。

其中，rcore-fs是OS可以用到的接口和实用工具。

对于VFS，会用到`FileSystem`和`INode`。

对于设备和缓冲层，会用到`BlockDevice`和`BlockCache`。

rcore-fs仅仅是对如下模块的包含：

## dev模块

设备接口层和块缓冲层实现

```rust
/// FS读、写的接口
pub trait Device: Send + Sync {
    fn read_at(&self, offset: usize, buf: &mut [u8]) -> Result<usize>;
    fn write_at(&self, offset: usize, buf: &[u8]) -> Result<usize>;
    fn sync(&self) -> Result<()>;
}

/// 它是与底层驱动程序之间的接口，属于设备接口层。定义了对块设备的操作（读和写）。
pub trait BlockDevice: Send + Sync {
    const BLOCK_SIZE_LOG2: u8;
    // 把块block_id写到缓冲区buf
    fn read_at(&self, block_id: BlockId, buf: &mut [u8]) -> Result<()>;
    // 把缓冲区buf写入到块block_id
    fn write_at(&self, block_id: BlockId, buf: &[u8]) -> Result<()>;
    fn sync(&self) -> Result<()>;
}
```



### block_cache模块

块缓冲层实现，采用LRU算法

```rust
/// 定义块缓存
pub struct BlockCache<T: BlockDevice> {
    device: T,				// 通过它进行块的读写
    bufs: Vec<Mutex<Buf>>,	// 代表内存中的缓冲区
    lru: Mutex<LRU>,
}
/// BlockCache的元数据
struct Buf {...}
enum BufStatus {...}

impl<T: BlockDevice> BlockCache<T> {
    pub fn new()	// 创建块缓存，把块上的数据读到缓冲区
    fn get_buf()	// 获取block_id对应的缓冲区地址
    fn _get_buf()
    fn get_unused()
    fn write_back()
}

impl<T: BlockDevice> Drop for BlockCache<T> {
    fn drop(&mut self) {
        BlockDevice::sync(self).expect("failed to sync");
    }
}

impl<T: BlockDevice> BlockDevice for BlockCache<T> {
    fn read_at()
    fn write_at()
    fn sync()
}

/// 双向链表的LRU算法实现
struct LRU {
    prev: Vec<usize>,
    next: Vec<usize>,
}
impl LRU {
    fn new()
    fn visit()
    fn victim()
    fn _list_remove()
    fn _list_insert_head()
}
```

### std_impl模块



## dirty模块

用于读写`dirty`标志。

## file模块

实现了`File`对象。

```rust
pub struct File {
    inode: Arc<dyn INode>,
    offset: usize,
    readable: bool,
    writable: bool,
}

impl File {
    pub fn new()
    pub fn read()
    pub fn write()
    pub fn info()
    pub fn get_entry()
}
```



## util模块

## vfs模块

```rust
/// 抽象的文件系统对象，代表了文件或目录
pub trait INode {}
impl dyn INode {}

/// INode的元数据
pub struct Metadata {...}
pub struct Timespec {...}
pub enum FileType {...}

/// FileSystem的元数据
pub struct FsInfo {...}
pub enum FsError {...}
impl fmt::Display for FsError {...}
impl From<DevError> for FsError {...}

/// 抽象的文件系统
pub trait FileSystem: Sync + Send {
    fn sync(&self) -> Result<()>;	// 同步数据到存储
    fn root_inode(&self) -> Arc<dyn INode>;	// 获取根索引节点
    fn info(&self) -> FsInfo;	// 获取文件系统的信息
}
```

# rcore-fs-sfs库

```rust
trait DeviceExt: Device {...}
impl DeviceExt for dyn Device {}

/// SFS的INode
pub struct INodeImpl {...}
impl Debug for INodeImpl {...}
impl INodeImpl {...}
impl vfs::INode for INodeImpl {...}
impl Drop for INodeImpl {...}

/// SFS的文件系统
pub struct SimpleFileSystem {...}
impl SimpleFileSystem {
    pub fn open() -> vfs::Result<Arc<Self>> {}	// 从设备载入SFS
    pub fn create
    fn wrap
    fn alloc_block
    fn free_block
    pub fn new_device_inode
    fn _new_inode
    fn get_inode
    ...
}
impl vfs::FileSystem for SimpleFileSystem {...}
impl Drop for SimpleFileSystem {...}

trait BitsetAlloc {
    fn alloc(&mut self) -> Option<usize>;
}
impl BitsetAlloc for BitVec<Lsb0, u8> {...}
impl AsBuf for BitVec<Lsb0, u8> {...}
impl AsBuf for [u8; BLKSIZE] {}
impl From<FileType> for vfs::FileType {...}
```

## strust子模块

描述磁盘上的结构

```rust
/// 超级块
pub struct SuperBlock {
    pub magic: u32,			// offset:0x0，必须是0x2f8dbe2b
    pub blocks: u32,		// offset:0x4
    pub unused_blocks: u32,	// offset:0x8
    pub info: Str32, // offset:0xc, 占32个字节，默认值"simple file system"
    pub freemap_blocks: u32,	// offset:0x2c
}

/// 磁盘上的索引结点
pub struct DiskINode {
    pub size: u32,	// 文件有多少个字节，对目录则无效
    pub type_: FileType,	// 1:文件，2:目录，3:符号链接，4:字符设备，5:块设备
    pub nlinks: u16,	// 硬链接数。包括"."和".."
    pub blocks: u32,	// 文件占多少个块
    pub direct: [u32; NDIRECT],	// 直接块的数组
    pub indirect: u32,	// 间接块的地址
    pub db_indirect: u32,	// 双间接块的地址
    pub device_inode_id: usize,	// 用于设备，记录(主设备号，次设备号)
    pub atime: Timespec,	// 最后访问时间
    pub mtime: Timespec,	// 最后修改时间
    pub ctime: Timespec,	// 最后改变时间
}

pub type DeviceINode = dyn vfs::INode;

pub struct IndirectBlock {
    pub entries: [u32; BLK_NENTRY],
}

/// 目录项，即目录文件的内容。一个目录项就代表了一个文件或子目录。
pub struct DiskEntry {
    pub id: u32,		// 索引结点号
    pub name: Str256,	// 文件名，占256个字符
}

impl SuperBlock {
        pub fn check()	// 检查魔数是否匹配
}

impl DiskINode {
    pub const fn new_file()
    pub const fn new_symlink()
    pub const fn new_dir()
    pub const fn new_chardevice()
}
```

# rcore-fs-hostfs库

```rust
/// 宿主机上的文件系统
pub struct HostFS {
    path: PathBuf,
    self_ref: Weak<HostFS>,
}

impl HostFS {
    pub fn new() -> Arc<HostFS> {}	// 创建HostFS
    fn wrap() -> Arc<Self> {}		// new方法用此方法把HostFS打包为Arc
}
```

# rcore-fs-sefs

简单加密文件系统

# rcore-fs-fuse程序

main执行过程

1. `env_log::init()`初始化日志系统。
2. 参数如果是zip则将`create`设为`true`表示创建镜像，如果是unzip则将`create`设为`false`表示解压镜像，如果是git-version则打印git commit号。
