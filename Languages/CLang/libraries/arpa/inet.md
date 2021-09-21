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

