#### 概述

网络插座。

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
/* 作用：对基于连接的网络插座有效(SOCK_STREAM, SOCK_SEQPACKET)。它会提取远程机器对sockfd的connect()请求，创建一个新的网络插座，返回值是这个新网络插座的文件描述符。
 */
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);

/* 作用：把地址addr绑定到网络插座sockfd。此操作即“给网络插座取名”。
 */
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);

/* 把sockfd连接到addr。让客户端建立到服务端的连接。 */
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);

/* 作用：把网络插座sockfd所绑定的地址记录到addr。此操作即“获取网络插座的名字”。
 */
int getsockname(int sockfd, struct sockaddr *addr, socklen_t *addrlen);

/* 获取网络插座sockfd的状态
 */
int getsockopt(int sockfd, int level, int optname, void *restrict optval, socklen_t *restrict optlen);

/* 让服务端的网络插座sockfd变成被动网络插座。(网络插座默认是主动)
 */
int listen(int sockfd, int backlog);

/* 把网络插座sockfd的信息读取到缓冲区buf */
ssize_t recv(int sockfd, void *buf, size_t len, int flags);

/* 把网络插座sockfd的信息读取到缓冲区buf
 * src_addr : 发送方的地址信息会记录在此
 * addrlen : 调用前，代表缓冲区src_addr的大小；返回后，被更新为src_addr所记录信息的实际大小。
 */
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
             struct sockaddr *src_addr, socklen_t *addrlen);

/* 把网络插座sockfd的信息记录到msg里 */
ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);

/* 向网络插座sockfd发送信息buf，len代表buf的长度。 */
ssize_t send(int sockfd, const void *buf, size_t len, int flags);

/* 向网络插座sockfd发送信息buf
 * len : buf的长度
 * dest_addr : 对方的地址信息。
 * addrlen : dest_addr的长度。
 * 返回值：成功，则返回发送的字节数；失败，返回-1，并设置errono。
 */
ssize_t sendto(int sockfd, const void *buf, size_t len, int flags,
             const struct sockaddr *dest_addr, socklen_t addrlen);

ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);

// 作用：创建连接的端点。其实就是依据一切皆是文件的思想，把网络映射为文件。
// 返回值：一个指向网络插座的文件描述符
// domain：定义连接域，即协议族。
//   AF_UNIX - 1, 即PF_UNIX，是PF_LOCAL的POSIX名，本地到主机(管道和文件域)
//   AF_FILE - 1, 即PF_FILE，是PF_LOCAL的另一个非标准名，本地到主机(管道和文件域)
//   AF_INET - 2, 即PF_INET，IP协议族
//   AF_INET6 - 10, 即PF_INET6, IPv6协议族
// type：网络插座的类型，也叫通信语义。
//   SOCK_STREAM - 提供顺序的、可靠的、双向的、基于连接的字节流。
//   SOCK_DGRAM - 支持数据报（无连接的，不可靠的，消息有最大长度）。
// protocol：指定网络插座要用的协议。通常一个网络插座的类型对应唯一的协议，此时此参数可指定为0；但若存在对应多个协议的情况，此时必须指定协议号。(详见/etc/protocols)

/* 设置网络插座sockfd的状态
 */
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);

int socket(int domain, int type, int protocol);

/* 作用：创建两个网络插座，并使它们之间互相连接。
 * 返回值：成功返回0，失败返回-1。
 * domain：定义连接域，即协议族。
 * type：网络插座的类型，也叫通信语义。
 *   SOCK_STREAM - 提供顺序的、可靠的、双向的、基于连接的字节流。
 *   SOCK_DGRAM - 支持数据报（无连接的，不可靠的，消息有最大长度）。
 * protocol：指定网络插座要用的协议。通常一个网络插座的类型对应唯一的协议，此时此参数可指定为0；但若存在对应多个协议的情况，此时必须指定协议号。
 * sv[2]：数组里的两个元素代表了新创建的两个网络插座。
 */
int socketpair(int domain, int type, int protocol, int sv[2]);

/* 关闭网络插座 */
int shutdown(int socket, int how);
```

