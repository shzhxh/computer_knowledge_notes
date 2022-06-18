#### 简介

*Advanced Configuration and Power Interface*，操作系统用它来查找和配置计算机硬件，让未使用的硬件休眠来进行电源管理，并进行状态监控。

##### RSDP

RSDP(Root System Description Pointer)是ACPI编程接口用到的数据结构。如下所示：

```c
struct acpi_table_rsdp {
    uint8_t signature[8]; /* "RSD PTR " */
    uint8_t checksum;     /* first 20 bytes must add up to 0 */
    uint8_t oem_id[6];
    uint8_t revision;	// ACPI的修订号，大的修定号向后兼容低的修订号。
    uint32_t rsdt_physical_address; /* 32-bit physical address of RSDT */
    uint32_t length;                /* table length */
    uint64_t xsdt_physical_address; /* 64-bit physical address of XSDT */
    uint8_t extended_checksum;      /* the entire table must add up to 0 */
    uint8_t reserved[3];
} __packed;
```

RSDP有两个可能的位置：一是在EBDA(Extended BIOS Data Area)的头1KB，EBDA是16位的实模式段，位于0x40e；一是在内存区域从0x000e0000到0x000fffff。

RSDT(Root System Description Table)是ACPI编程接口用到的数据结构。

XSDT(eXtended System Descriptor Table)是RSDT的64位版本。