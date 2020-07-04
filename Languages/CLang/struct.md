C99支持成员变量前加点来初始化结构体：

```c
#include <stdio.h>

struct test {
    int a;
    int b;
}

int main(void){
    struct test t1 = {
        .a = 1,
        .b = 0
    };
    printf("t1.a = %d, t1.b = %d\n",t1,a, t1,b);
}
```



结构体中冒号的作用：

* 位域：冒号后面的数字表示冒号前面的变量占的位数
