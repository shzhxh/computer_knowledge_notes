#### 概述

跳转

#### 相关数据结构

```c
struct __jmp_buf_tag
  {
    __jmp_buf __jmpbuf;		/* Calling environment.  */
    int __mask_was_saved;	/* Saved the signal mask?  */
    __sigset_t __saved_mask;	/* Saved signal mask.  */
  };

typedef struct __jmp_buf_tag jmp_buf[1];
```

#### 相关函数

```c
/*
 * 作用：把调用环境保存到env里（调用环境一般包含栈指针、指令指针、相关寄存器的值和信号掩码）。返回0。这个env会在随后的longjmp函数里用到。
 */
int setjmp(jmp_buf env);
int sigsetjmp(sigjmp_buf env, int savesigs);
/*
 * 作用：使用env中的信息将控制转移回调用setjmp的位置。
 * val:使setjmp返回val。
 */
void longjmp(jmp_buf env, int val);
void siglongjmp(sigjmp_buf env, int val);
```

