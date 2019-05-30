[HybridOS](https://github.com/FMSoftCN/hybridos)

#### 概述

- 面向物联网的开源操作系统。
- 引入区块链技术解决物联网的安全问题。
- 基于Linux。

#### 架构

尝试为从云到客户端的所有设备提供完整的软件栈和协议栈。

##### 设备端

系统组件

- 满足POSIX标准的内核：如Linux
- 基本的运行时工具：如Busybox
- 通用系统服务
- HybridOS服务

守护进程

- 日志服务
- 安全服务

服务

- WebSocket
- HTTP
- CoAP
- 流服务
- MQTT　Broker

系统软件结构图

- 应用层
- 框架层：hiWebKit与HFCL、V8引擎和hybrid.js
- 服务层：python运行时，服务，MiniGUI
- 守护进程
- 标准库、硬件抽象层等
- 内核与驱动

##### 客户端

客户端程序在各OS平台可以运行的核心在**hiWebKit**，它继承了**WebKit**的特性。

**HybridOS程序引擎**(HAE)是对HFCL和hiWebKit的封装，是HVML(HybridOS可视化标记语言)和HTML5的用户代理。

客户端程序即可以直接在HAE上运行，也可以通过HAE SDK打包成一个独立程序。

##### 云端

通过在云上提供新的服务来解决物联网在云计算环境中的问题。

- 设备验证与注册。
- 固件在线升级。
- 应用与服务在线升级。
- MQTT(消息队列遥测传输)服务。
- 身份验证服务。

运行在云端的上述服务与运行在设备端的HybridOS安全服务紧密合作，以确保物联网应用程序的安全性。