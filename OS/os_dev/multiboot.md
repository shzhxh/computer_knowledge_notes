multiboot是一个启动规范。

#### OS映像格式

OS映像必须包含一个multiboot头，multiboot头必须在OS映像的前8192字节且必须32位对齐。

```c
struct multiboot_header {
    uint32_t magic;
    uint32_t flags;
    uint32_t checksum;
    /* valid if MULTIBOOT_AOUT_KLUDGE is set */
    uint32_t header_addr;
    uint32_t load_addr;
    uint32_t load_end_addr;
    uint32_t bss_end_addr;
    uint32_t entry_addr;
    /* valid if MULTIBOOT_VIDEO_MODE is set */
    uint32_t mode_type;
    uint32_t width;
    uint32_t height;
    uint32_t depth;
} __attribute__((packed));
```



#### 机器状态

EAX必须包含魔数0x2BADB002

EBX必须包含由引导程序提供的引导信息的物理地址(即`multiboot_info`)

A20 gate必须已经开启

#### 引导信息格式

```c
struct multiboot_info {
    uint32_t flags;		// 不同的位标志了本数据结构哪些数据是有效的，0:mem_*, 1:bot_device, 2:cmdline, 3:mods_*, 4或5:syms, 6:map_*, 7:drives_*, 8:config_table, 9:boot_loader_name, 10:amp_table, 11:vbe_*。
    uint32_t mem_lower;
    uint32_t mem_upper;
    uint32_t boot_device;
    uint32_t cmdline;
    uint32_t mods_count;
    uint32_t mods_addr;
    union {
        struct multiboot_aout_symbol_table aout_sym;
        struct multiboot_elf_section_header_table elf_sec;
    };
    uint32_t mmap_length;
    uint32_t mmap_addr;
    uint32_t drives_length;
    uint32_t drives_addr;
    uint32_t config_table;
    uint32_t boot_loader_name;
    uint32_t apm_table;
    uint32_t vbe_control_info;
    uint32_t vbe_mode_info;
    uint16_t vbe_mode;
    uint16_t vbe_interface_seg;
    uint16_t vbe_interface_off;
    uint16_t vbe_interface_len;
} __packed;
```

- 由`multiboot_info->flags`的第6位决定的mmap_*域，其保存的是内存分布情况的信息，其实就是一个数组，mmap_length是数组长度，mmap_addr是数组地址。数组中每项的内容如下：

  ```c
  struct multiboot_mmap_entry {
      uint32_t size;	// addr+len+type占用的字节数，可能大于20，但一定不会低于20
      uint64_t addr;	// 内存区域起始地址
      uint64_t len;	// 内存区域大小
      uint32_t type;	// 内存区域类型，1为可用，其它则是保留区域
  } __packed;
  ```

  