```c
struct __jmp_buf_tag
  {
    __jmp_buf __jmpbuf;		/* Calling environment.  */
    int __mask_was_saved;	/* Saved the signal mask?  */
    __sigset_t __saved_mask;	/* Saved signal mask.  */
  };

typedef struct __jmp_buf_tag jmp_buf[1];
```



函数

```c
/*
 * 保存ENV里的调用环境，也会保存信号掩码。返回0.
 */
int setjmp (jmp_buf);
_Noreturn void longjmp (jmp_buf, int);
```

