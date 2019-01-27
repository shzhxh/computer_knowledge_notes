### arch/riscv

##### kernel/setup.c

`setup_arch`：被外部的`start_kernel`(init/main.c)调用。设置内存与多处理器。

##### kernel/traps.c

`trap_init`：被外部的`start_kernel`（init/main.c)调用。被内部的`smp_callin`(kernel/smpboot.c)调用。设置sscratch为0,告诉异常处理程序我们在内核中；为 stvec指定异常处理程序`handle_exception`(kernel/entry.S); 设置sie寄存器为-1使能所有的中断。

##### kernel/smpboot.c

`setup_smp`:被setup_arch调用。设置多处理器。

##### kernel/entry.S

`__switch_to`:通过被`switch_to`(asm/switch_to.h)封装的形式被外部的`context_switch` (kernel/sched/core.c)调用。

`SAVE_ALL`：宏，在进入系统调用或异常前将所有的寄存器保存在栈里。

`RESTORE_ALL`：宏，在从系统调用或异常返回前将栈中的值再保存到寄存器里。

`handle_exception`：入口，异常处理程序。

`ret_form_fork`：入口，从fork返回。

`ret_from_kernel_thread`：入口，从内核线程中返回。

`__fstate_save`：入口，文件状态保存。

`__fstate_restore`：入口，文件状态重新保存。

`excep_vect_table`：入口，异常向量表。

##### kernel/head.S

`_start`：是整个内核的入口。

