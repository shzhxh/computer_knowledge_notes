#### 概述

ELF是一种文件格式，可以通过`man elf`命令获取到关于elf的详细信息。

ELF文件 = ELF header + file data  
file data = program header + section header + data

ELF header的位置是在文件偏移为0的地方，program header table和section header table在文件中的偏移位置由ELF header指定，以上两个表又描述了文件其它数据的信息。

#### 一个32位ELF文件实例
```c
struct elfhdr{
7f45 4c46	uint32_t e_magic;	// 0x7f+"ELF"
01		uint8_t  ei_class;	// 1: 32 bit format, 2: 64 bit format
01		uint8_t  ei_data;	// 1: little endianness, 2: big endianness
01		uint8_t  ei_version;	// 1: original version
00		uint8_t  ei_osabi;	// Application Binary Interface
00		uint8_t  ei_abiversion;
0000 0000 0000 00	uint8_t  ei_pad[7];	// currently unused
0200		uint16_t e_type;	// 1:reloc,2:excut,3:shared,4:core
0300		uint16_t e_machine;	// 0x03:x86
0100 0000	uint32_t e_version;	// 1: original version
1083 0408	uint32_t e_entry;	// virtual address of entry point (uint64_t if 64 bit format)
3400 0000	uint32_t e_phoff;	// offset of the program header table (0x34 for 32 bit)
a41a 0000	uint32_t e_shoff;	// offset of the section header table
0000 0000	uint32_t e_flags;	// architecture-specific flags,usually 0
3400		uint16_t e_ehsize;	// size of this header, 52 for 32-bit format
2000		uint16_t e_phentsize;	// size of program headers
0900		uint16_t e_phnum;	// num of entries in the program header table
2800		uint16_t e_shentsize;	// size of section headers
2400		uint16_t e_shnum;	// num of  section headers
2100		uint16_t e_shstrndx;	// index of the section header table entry
};

struct proghdr{
0600 0000	uint32_t p_type;	// 6:PT_PHDR
3400 0000	uint32_t p_offset;	// offset in the file image
3480 0408	uint32_t p_vaddr;	// virtual address
3480 0408	uint32_t p_paddr;	// physical address
2001 0000	uint32_t p_filesz;	// size in bytes in file image
2001 0000	uint32_t p_memsz;	// size in bytes in memory
0500 0000	uint32_t p_flags;	// read/write/execute bits
0400 0000	uint32_t p_align;	// alignment
};
```

#### 一个64位ELF文件实例
```c
struct elfhdr{
7f45 4c46	uint32_t e_magic;	// 0x7f+"ELF"
02			uint8_t  ei_class;	// 1: 32 bit format, 2: 64 bit format
01			uint8_t  ei_data;	// 1: little endianness, 2: big endianness
01			uint8_t  ei_version;	// 1: original version
00			uint8_t  ei_osabi;	// Application Binary Interface
00			uint8_t  ei_abiversion;
0000 0000 0000 00	uint8_t  ei_pad[7];	// currently unused
0200 	uint16_t e_type;	// 1:reloc,2:excut,3:shared,4:core
f300 	uint16_t e_machine;	// 0xf3:unknown
0100 0000	uint32_t e_version;	// 1: original version
b000 0100 0000 0000	uint64_t e_entry;	// virtual address of entry point:0x100b0
4000 0000 0000 0000	uint64_t e_phoff;	// offset of the program header table (0x40 for 64 bit)
286b 0300 0000 0000	uint64_t e_shoff;	// offset of the section header table:224040(bytes)
0400 0000	uint32_t e_flags;	// architecture-specific flags:4
4000  		uint16_t e_ehsize;	// size of this header, 64 for 64-bit format
3800		uint16_t e_phentsize;	// size of program headers:56(bytes)
0200		uint16_t e_phnum;	// num of entries in the program header table:2
4000		uint16_t e_shentsize;	// size of section headers:64(bytes)
1700 		uint16_t e_shnum;	// num of  section headers:23
1600		uint16_t e_shstrndx;	// index of the section header table entry:22
};

struct proghdr{
0100 0000	uint32_t p_type;	// 1:PT_LOAD
0500 0000	uint32_t p_flags;	// read/write/execute bits:R,E
0000 0000 0000 0000	uint64_t p_offset;	// offset in the file image:0
0000 0100 0000 0000	uint64_t p_vaddr;	// virtual address:0x10000
0000 0100 0000 0000	uint64_t p_paddr;	// physical address:0x10000
d439 0000 0000 0000	uint64_t p_filesz;	// size in bytes in file image:0x39d4
d439 0000 0000 0000	uint64_t p_memsz;	// size in bytes in memory:0x39d4
0010 0000 0000 0000	uint64_t p_align;	// alignment:1000
};
```
