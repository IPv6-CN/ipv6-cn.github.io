---
layout: post
title: "好书推荐——《IPv6 精要第二版》"
categories: [book]
---
[本书出版社链接](http://www.ciscopress.com/store/ipv6-fundamentals-a-straightforward-approach-to-understanding-9781587144776
)，
[样章下载](http://www.ciscopress.com/content/images/9781587144776/samplepages/9781587144776_CH04.pdf
)（含第四章和目录）

# 本书目录

## 第一章 IPv6 简介
本章介绍了为什么今天的互联网需要一个新的网络层协议——IPv6，来满足用户的需求。还分析了 IPv4 的限制。描述了 IPv6 如何解决这些问题。同时提供了其他的好处。

本章还简要介绍了 IPv4 和 IPv6 的历史。还讨论了 IPv4 迁移技术 CIDR 和地址转换（NAT）。
## 第二章 IPv6 Primer
本章介绍了一些基础概念和协议，这些内容将在本书的后面更为详细的描述，包括 IPv6 地址类型，动态地址分配基础，用来表示 IPv6 地址的十六进制数字系统。本章还介绍了 IPv6 的与众不同之处。
## 第三章 比较 IPv4 和 IPv6
本章比较和对照了 IPv4 和 IPv6 两种协议。还解释了碎片如何处理。还讨论了 IPv6 扩展包头。
## 第四章 IPv6 地址表示和地址类型
本章介绍了 IPv6 寻址和地址类型。它讨论了 IPv6 地址的表示形式，以及表示 IPv6 地址的不同格式和压缩 IPv6 表示法的规则。

本章简要介绍不同类型的 IPv6 地址，包括单播，多播和任播。它还讨论了前缀长度表示法。
## 第五章 全局单播地址
本章详细分析了全局单播地址。它分析了全局单播地址的不同部分, 演示了在 Cisco IOS 和主机操作系统上的手动配置全局单播地址。

本章还介绍了 IPv6 的子网划分以及前缀分配。
## 第六章 链路本地单播地址
本章分析了链路本地地址，包括静态和动态 链路本地地址 配置示例。还解释了 EUI-64 流程，以及 IPv6 中链路本地地址的重要性。
## 第七章 多播地址
本章将研究多播地址，包括众所周知（well-known）的和请求节点（solicited-node）的多播。讨论了多播地址优于广播地址（广播地址在 IPv6 中不存在）之处，以及 IPv6 多播地址如何映射到以太网 MAC 地址。
## 第八章 IPv6 中的动态寻址基础
本章介绍并比较动态地址分配的三种方法：无状态地址自动配置（SLAAC），无状态 DHCPv6 和有状态 DHCPv6。后面的章节将更详细地讨论这些方法。
## 第九章 无状态地址自动配置（SLAAC）
本章详细讨论 SLAAC 过程。包括一个使用 Wireshark 分析 ICMPv6 路由器宣告消息（这个信息是 SLAAC）的例子。

本章讨论如何将隐私扩展和临时地址与 SLAAC 生成的地址一起使用，包括不同的状态和生命周期。还解释了如何管理主机操作系统上的隐私选项。
## 第十章 无状态 DHCPv6
本章介绍了 SLAAC 和其他无状态 DHCPv6 服务。它涵盖了 DHCPv6 术语和消息类型，以及客户端和服务器之间的 DHCPv6 进程。本章解释了 rapid-commit 选项和中继代理。
## 第十一章 有状态的 DHCPv6
本章介绍了有状态的 DHCPv6 服务，类似于 IPv4 的 DHCP。它还介绍了带前缀下发的 DHCPv6，这是一种为家庭提供 IPv6 地址空间的常用方法。
## 第十二章 ICMPv6
本章介绍了 ICMPv6，它是比 ICMPv4 更健壮的协议。本章涵盖了 ICMPv6 错误消息，包括目标不可达（Destination Unreachable），分组太大（Packet Too Big），超时（Time Exceeded）和参数问题（Parameter Problem）。

还介绍了提供信息的 ICMPv6 Echo Request 和 Echo Reply 消息以及 Multicast Listener Discovery 消息。
## 第十三章 ICMPv6 的邻居发现
本章讨论 ICMPv6 邻居发现，包括路由器请求（RS：Router Solicitation），路由器通告（RA：Router Advertisement），邻居请求（NS：Neighbor Solicitation），邻居通告（NA：Neighbor Advertisement）和重定向（Redirect）消息。

IPv6 不仅解决了更大的地址空间问题，而且通过带使用邻居发现协议的 ICMPv6 使得网络操作方面发生了重大变化，包括链路层地址解析（类似 IPv4 中的 ARP），重复地址检测（DAD），无状态地址自动配置（SLAAC）和邻居不可达检测（NUD）。

本章讨论了 IPv6 邻居缓存和邻居缓存状态，类似于 IPv4 ARP 缓存。
## 第十四章 IPv6 路由表和静态路由
本章检视了 IPv6 路由表。还讨论了 IPv6 静态路由的配置，类似于 IPv4 的静态路由。它解释了 IPv6 默认路由和路由汇总，以及 IPv6 的 CEF。
## 第十五章 用于 IPv6 的 EIGRP
本章讨论用于 IPv6 的 EIGRP。首先比较了 IPv4 的 EIGRP 和 IPv6 的 EIGRP。还讨论了用于 IPv6 的经典 EIGRP 和 EIGRP 命名模式（适用于 IPv4 和 IPv6）的配置和验证。
## 第十六章 OSPF 第三版
本章讨论了 OSPFv3。它首先比较了 OSPFv2（仅支持 IPv4），传统 OSPFv3（仅支持 IPv6）和带地址类型支持的 OSPFv3（同时可用于 IPv4 和 IPv6）的比较。还讨论了对传统 OSPFv3 和带地址类型支持的 OSPFv3 的配置和验证。
## 第十七章 在网络中部署 IPv6
本章介绍部署 IPv6 的策略，包括创建 IPv6 地址规划，配置 IPv6 VLAN 以及在第一跳路由器上使用 ICMPv6 或第一跳冗余协议实现透明故障转移（FHRP）。

本章还讨论 IPv4 和 IPv6 集成和共存，包括双栈，NAT64 和隧道。
