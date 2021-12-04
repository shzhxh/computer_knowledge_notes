#### 概述

ip地址形式之间的转化。ip地址的形式可能是点分十进制，二进制，文本。

用到的数据结构，在`netinet/in.h`：

```c
/* Internet address.  */
typedef uint32_t in_addr_t;
struct in_addr
  {
    in_addr_t s_addr;
  };
```

#### 相关函数

```c
/* 把ipv4地址从文本转为二进制 */
in_addr_t inet_addr (const char *);
in_addr_t inet_network (const char *);
/* 把ipv4地址从十进制转为点分十进制 */
char *inet_ntoa (struct in_addr);
/* 把ip地址从文本转为二进制 */
int inet_pton (int, const char *__restrict, void *__restrict);
/* 把ip地址从二进制转为文本 */
const char *inet_ntop (int, const void *__restrict, char *__restrict, socklen_t);
```

