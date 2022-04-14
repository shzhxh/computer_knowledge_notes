#### 简介

浮点环境

#### 宏

```c
FE_DIVBYZERO	// 除零异常
FE_INEXACT		// 不精确的异常
FE_INVALID		// 无效参数异常
FE_OVERFLOW		// 上溢错误
FE_UNDERFLOW	// 下溢错误
```



#### 函数

```c
/* 清除异常 */
int feclearexcept(int excepts);
int fegetexceptflag(fexcept_t *flagp, int excepts);
int feraiseexcept(int excepts);
int fesetexceptflag(const fexcept_t *flagp, int excepts);
/* 返回异常 */
int fetestexcept(int excepts);

int fegetround(void);
/* 设置舍入模式 */
int fesetround(int rounding_mode);

int fegetenv(fenv_t *envp);
int feholdexcept(fenv_t *envp);
/* 从envp保存浮点环境 */
int fesetenv(const fenv_t *envp);
int feupdateenv(const fenv_t *envp);

```

