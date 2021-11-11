```c
// 作用：创建连接的端点
// 返回值：一个指向网络插座的文件描述符
// domain：定义连接域，即协议族。
//   AF_INET - IPv4协议
// type：网络插座的类型，也叫通信语义。
//   SOCK_STREAM - 提供顺序的、可靠的、双向的、基于连接的字节流。
//   SOCK_DGRAM - 支持数据报（无连接的，不可靠的，消息有最大长度）。
// protocol：指定网络插座要用的协议。通常一个网络插座的类型对应唯一的协议，此时此参数可指定为0；但若存在对应多个协议的情况，此时必须指定协议号。
int socket(int domain, int type, int protocol);
```

结构体

```c
/* 网络插座的地址  */
struct sockaddr  {
    __SOCKADDR_COMMON (sa_);    /* 通用数据: 地址族和长度  */
    // 注，上一行展开后是：unsigned short int sa_family;
    char sa_data[14];           /* 地址数据，包含了端口号和地址  */
  };

/* Internet网络插座的地址  */
struct sockaddr_in  {
    __SOCKADDR_COMMON (sin_);
    // 注，上一行展开后是：unsigned short int sin_family;
    in_port_t sin_port;                 /* 端口号  */
    // 注，in_port_t即uint16_t
    // 注，可以使用htons()把端口号由主机字节序转换为网络字节序
    struct in_addr sin_addr;            /* 地址  */
    // 注，inet_addr()可以把IP地址的点分十进制形式转换为网络字节序
    // 注，htonl()可以把IP地址由主机字节序转换为网络字节序
    /* sin_zero是为了和`struct sockaddr'对齐，仅占位不使用  */
    unsigned char sin_zero[sizeof (struct sockaddr)
                           - __SOCKADDR_COMMON_SIZE
                           - sizeof (in_port_t)
                           - sizeof (struct in_addr)];
  };
// 关于struct in_addr的定义
typedef uint32_t in_addr_t;
struct in_addr  {
    in_addr_t s_addr;
  };
```
