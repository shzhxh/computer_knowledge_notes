#### fat32的磁盘结构

FAT文件系统=引导记录(512字节) + 文件分配表 + 根目录 + 数据区

#### 引导记录

引导记录=BIOS参数块(0x00-0x23) + 扩展引导记录(0x24-0x1ff)

FSInfo结构体占用512字节，扇区号在扩展引导记录里指定。

```rust
pub(crate) struct BootSector {
    bootjmp: [u8; 3],		// offset:0x00
    oem_name: [u8; 8],		// offset:0x03
    pub(crate) bpb: BiosParameterBlock,
    boot_code: [u8; 448],	// offset:0x5a
    boot_sig: [u8; 2],		// offset:0x1fe, 启动分区的签名"0xAA55"
}
pub(crate) struct BiosParameterBlock {
    pub(crate) bytes_per_sector: u16,	// offset:0x0b
    pub(crate) sectors_per_cluster: u8,	// offset:0x0d
    pub(crate) reserved_sectors: u16,	// offset:0x0e
    pub(crate) fats: u8,	// offset:0x10, 文件分配表的个数，一般是2
    pub(crate) root_entries: u16,		// offset:0x11, 根目录包含多少个条目
    pub(crate) total_sectors_16: u16,	// offset:0x13
    pub(crate) media: u8,				// offset:0x15
    pub(crate) sectors_per_fat_16: u16,	// offset:0x16
    pub(crate) sectors_per_track: u16,	// offset:0x18
    pub(crate) heads: u16,				// offset:0x1a
    pub(crate) hidden_sectors: u32,		// offset:0x1c
    pub(crate) total_sectors_32: u32,	// offset:0x20

    // Extended BIOS Parameter Block
    pub(crate) sectors_per_fat_32: u32,	// offset:0x24
    pub(crate) extended_flags: u16,		// offset:0x28
    pub(crate) fs_version: u16,			// offset:0x2a
    pub(crate) root_dir_first_cluster: u32,	// offset:0x2c, 根目录的簇号
    pub(crate) fs_info_sector: u16,		// offset:0x30, FSInfo的扇区号
    pub(crate) backup_boot_sector: u16,	// offset:0x32
    pub(crate) reserved_0: [u8; 12],	// offset:0x34
    pub(crate) drive_num: u8,			// offset:0x40
    pub(crate) reserved_1: u8,			// offset:0x41
    pub(crate) ext_sig: u8,				// offset:0x42, 只能是0x28或0x29
    pub(crate) volume_id: u32,			// offset:0x43
    pub(crate) volume_label: [u8; 11],	// offset:0x47
    pub(crate) fs_type_label: [u8; 8],	// offset:0x52, 总是"FAT32"
}

struct FsInfoSector {
    free_cluster_count: Option<u32>,	// offset:0x1e8
    next_free_cluster: Option<u32>,		// offset:0x1ec
    dirty: bool,
}
impl FsInfoSector {
    const LEAD_SIG: u32 = 0x4161_5252;	// 先导签名的值，offset:0x0
    const STRUC_SIG: u32 = 0x6141_7272;	// 中间签名的值，offset:0x1e4
    const TRAIL_SIG: u32 = 0xAA55_0000;	// 结尾签名的值，offset:0x1fc
    
    ...
}
```



#### 文件分配表

文件分配表里的元素为28字节大小，代表了当前文件下一个簇的编号。

当元素值大于等于0x0ffffff8，代表没有多余的簇了，即整个文件已读取完成。

当元素值等于0x0ffffff7，代表这个簇是坏的。

当元素值等于0，代表这个簇是空闲的。所以0号元素是被保留的。

1号元素是被保留的，留做将来使用。其值必须是0xffffffff。

#### 根目录

记录了整个磁盘的结构。是由目录条目组成的数组，数组元素为32字节大小。

```rust
pub(crate) struct DirFileEntryData {
    name: [u8; SFN_SIZE],	// offset:0, SFN_SIZE值为11.
    attrs: FileAttributes,	// offset:11, u8类型。可以决定当前条目是目录还是文件。
    reserved_0: u8,			// offset:12
    create_time_0: u8,		// offset:13
    create_time_1: u16,		// offset:14
    create_date: u16,		// offset:16
    access_date: u16,		// offset:18
    first_cluster_hi: u16,	// offset:20, 当前文件第一个簇的簇号的高16位。
    modify_time: u16,		// offset:22
    modify_date: u16,		// offset:24
    first_cluster_lo: u16,	// offset:26, 当前文件第一个簇的簇号的低16位。
    size: u32,				// offset:28, 当前文件占用了多少字节。
}
```

#### rust-fs库

```rust
mod log_macros;

mod boot_sector;
mod dir;
mod dir_entry;
mod error;
mod file;
mod fs;
mod io;
mod table;
mod time;

pub use crate::dir::*;
pub use crate::dir_entry::*;
pub use crate::error::*;
pub use crate::file::*;
pub use crate::fs::*;
pub use crate::io::*;
pub use crate::time::*;
```

##### fs包

```rust
pub struct FileSystem<IO: ReadWriteSeek, TP, OCC> {
    pub(crate) disk: RefCell<IO>,
    pub(crate) options: FsOptions<TP, OCC>,
    fat_type: FatType,
    bpb: BiosParameterBlock,
    first_data_sector: u32,
    root_dir_sectors: u32,
    total_clusters: u32,
    fs_info: RefCell<FsInfoSector>,
    current_status_flags: Cell<FsStatusFlags>,
}

impl<IO: Read + Write + Seek, TP, OCC> FileSystem<IO, TP, OCC> {
	pub fn new()	// 创建一个文件系统的实例
    ...
    pub fn root_dir()	// 返回根目录的对象
}
```

