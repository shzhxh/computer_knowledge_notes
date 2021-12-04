#### 概述

网络插座。

协议族有：

- AF_UNIX - 1, 即PF_UNIX，是PF_LOCAL的POSIX名，本地到主机(管道和文件域)
- AF_FILE - 1, 即PF_FILE，是PF_LOCAL的另一个非标准名，本地到主机(管道和文件域)
- AF_INET - 2, 即PF_INET，IP协议族
- AF_INET6 - 10, 即PF_INET6, IPv6协议族

#### 相关结构体

```c
/* 网络插座的地址。定义在bits/socket.h  */
struct sockaddr  {
    __SOCKADDR_COMMON (sa_);    /* 通用数据: 地址族和长度  */
    // 注，上一行展开后是：unsigned short int sa_family;
    char sa_data[14];           /* 地址数据，包含了端口号和地址  */
  };

/* Internet网络插座的地址。定义在netinet/in.h  */
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

// Internet address。定义在netinet/in.h
typedef uint32_t in_addr_t;
struct in_addr  {
    in_addr_t s_addr;
  };
```



#### 相关函数

```c
// 作用：创建连接的端点
// 返回值：一个指向网络插座的文件描述符
// domain：定义连接域，即协议族。
// type：网络插座的类型，也叫通信语义。
//   SOCK_STREAM - 提供顺序的、可靠的、双向的、基于连接的字节流。
//   SOCK_DGRAM - 支持数据报（无连接的，不可靠的，消息有最大长度）。
// protocol：指定网络插座要用的协议。通常一个网络插座的类型对应唯一的协议，此时此参数可指定为0；但若存在对应多个协议的情况，此时必须指定协议号。
int socket(int domain, int type, int protocol);
/*
 * 作用：创建两个网络插座，并使它们之间互相连接。
 * 返回值：成功返回0，失败返回-1。
 * domain：定义连接域，即协议族。
 * type：网络插座的类型，也叫通信语义。
 *   SOCK_STREAM - 提供顺序的、可靠的、双向的、基于连接的字节流。
 *   SOCK_DGRAM - 支持数据报（无连接的，不可靠的，消息有最大长度）。
 * protocol：指定网络插座要用的协议。通常一个网络插座的类型对应唯一的协议，此时此参数可指定为0；但若存在对应多个协议的情况，此时必须指定协议号。
 * sv[2]：数组里的两个元素代表了新创建的两个网络插座。
 */
int socketpair(int domain, int type, int protocol, int sv[2]);
```

