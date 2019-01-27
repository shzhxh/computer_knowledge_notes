#### 虚拟内存管理之数据结构

详见vmm.c和vmm.h。

##### struct mm_struct

描述进程在内存中分布的情况，是进程内存管理的核心数据结构  
描述：控制具有相同PDT的vma的结构体  
mmap_list：用于由vma组成的双向链表  
mmap_cache：当前访问的vma的指针，用于快速访问  
pgdir：这些vma的PDT的指针  
map_count：这些vma的数量  
sm_priv：swap manager的私有数据  
mm_count：共享此mm的进程的数量  
mm_sem：使用dup_mmap函数复制此mm的互斥锁  
locked_by：拥有互斥锁的进程的pid

##### struct vma_struct

虚拟内存区域(vma)的结构体  
  vm_mm：vma所在mm_struct结构体的指针 

  vm_start：vma的起始地址  
  vm_end：vma的结束地址  
  vm_flags：包含VM_READ,VM_WRITE,VM_EXEC,VM_STACK四种标志  
  list_link：用于将vma_struct串成双向链表

##### mm_create  

struct mm_struct *mm_create(void)

为mm_struct结构体申请内存空间并初始化，返回mm_struct结构体的指针。

##### mm_destroy

void mm_destroy(struct mm_struct *mm)

释放mm_struct结构体及其包含的vma_struct结构体的内存空间，并将mm赋值为NULL。

##### find_vma  

struct vma_struct *find_vma(struct mm_struct *mm, uintptr_t addr)

输入mm_struct结构体mm和地址addr，返回addr所在的vma_struct结构体的指针。依次遍历mm内的vma，直到找到addr>=vm_start且addr<=vm_end的vma，将其指针赋值给mm->mmap_cache，并返回其指针值。如果遍历完vma的链表依然找不到满足条件的vma，则仅返回NULL。

##### vma_create  

struct vma_struct *vma_create(uintptr_t vm_start, uintptr_t vm_end, uint_t vm_flags)

为vma_struct结构体申请内存空间并初始化，返回其指针。

##### insert_vma_struct  

void insert_vma_struct(struct mm_struct *mm, struct vma_struct *vma)

输入mm_struct结构体mm和vma_struct结构体vma，将vma插入到mm所管理的vma_struct链表里，无返回值。vma_struct的链表里是依据vm_start从小到大排序的，所以要找到合适的位置将vma插入。

##### mm_map

输入mm_struct结构体mm，地址addr，长度len，标志vm_flags，vma_struct结构体指针的指针vma_store。为起始地址为addr，长度为len的内存区域创建vma，将vma链接到mm所管理的vma_struct链表里。如果vma_store非NULL,则将所得vma的指针赋值给vma_store。返回值为0表示函数正确执行，返回值非0则为错误代码。

##### mm_count_inc  

输入mm_struct结构体的指针mm,将mm->mm_count加1,返回其值。意指共享此mm的进程增加了1个。

##### setup_pgdir

输入mm_struct结构体的指针mm，申请一个页作为PDT，将此页的虚拟地址赋值给mm->pgdir。如果函数执行正确则返回0,否则返回错误码。

##### load_icode_read

输入fd，地址buf，长度len，偏移offset，将文件中位置offset，长度len的数据读到内存buf处。如果函数执行正确则返回0,否则返回错误码。

##### user_mem_check

`bool user_mem_check(struct mm_struct *mm, uintptr_t addr, size_t len, bool write)`

查询从addr开始，长度为len的虚拟内存区域是否在mm的范围内。

#### 虚拟内存管理之Page Fault异常处理

详见vmm.c。

调用关系：__alltraps->trap->exception_handler->pgfault_handler->do_pgfault

##### do_pgfault

int do_pgfault(struct mm_struct *mm, uint_t error_code, uintptr_t addr)

首先通过mm找到addr所对应的vma，如果没找到则这是一个地址无效的错误，将直接退出。后续要用到的addr所对应页的权限则来自于vma->vm_flags。然后通过mm->pgdir找到addr所对应的ptep。如果\*ptep为0则创建addr到某物理页的映射。如果\*ptep不为0则表明是此页被换出到硬盘了，则会把此页换回到内存，并创建到addr的映射。

#### 虚拟内存管理之页置换

Ucore使用的页置换算法是FIFO，详见swap_fifo.c。

页置换函数详见swap.c。

#### 物理内存管理之算法：first fit

Ucore的物理内存管理采用了first fit算法，详见kern/mm/default_pmm.c。

First fit算法是把空闲的内存块放在一个链表里，当有一个内存请求发过来时，会依次遍历内存里的每一项，直到找到一个足够大的内存块，并把它分配出去。如果找到的这个内存块远大于要请求的大小，则会将此内存块拆分，然后把剩下的部分再放入链表里。

#### 物理内存管理之分页

详见pmm.c。

##### get_pte

pte_t *get_pte(pde_t *pgdir, uintptr_t la, bool ceate)

给出页目录pgdir和虚拟地址la，返回la所在物理页的地址的指针。是否创建页表还是单纯的查询，这由第三个参数create决定。**注意**：该指针只是用来放虚拟地址对应的物理页的，并不表明该指针里已经放了该物理页的地址。

##### get_page

struct Page *get_page(pde_t *pgdir, uintptr_t la, pte_t **ptep_store)

给出页目录pgdir和虚拟地址la，返回代表物理页的页结构体的指针。由于物理页都放在链表里，而页结构体里放着链表的地址，所以可以通过页结构体找到物理页。第三个参数是存放**指针的指针**，如果它不为NULL的话，用来存放la所在物理页的指针。

##### page_remove_pte

void page_remove_pte(pde_t *pgdir, uintptr_t la, pte_t *ptep)

给出页目录pgdir，虚拟地址la,虚拟地址la所在物理页的地址的指针ptep，将*ptep归零。本函数会给ptep里存放的物理页引用数减一，如果减一后引用数变为0,则会**释放**该页。**注意**：pgdir和la这两个参数在本函数里**没有任何作用**，应去掉。

##### page_remove

void page_remove(pde_t *pgdir, uintptr_t la)

其实就是page_remove_pte函数，不同的是ptep是通过pgdir和la两个参数来确定的。

##### page_insert

int page_insert(pde_t *pgdir, struct Page *page, uintptr_t la, uint32_t perm)

通过把page所对应的物理页作为la的页表项的办法，建立la到page的映射，该页表项的权限即为perm。

##### pgdir_alloc_page

struct Page *pgdir_alloc_page(pde_t *pgdir, uintptr_t la, uint32_t perm)

其实就是page_insert函数。如果swap_init_ok不为0的话，还添加了swap_map_swappable函数的功能。