---
layout: post
title:  "IPv6 安全隐患的第一大来源"
excerpt: "是在不理解的情况下就部署了 IPv6，无计划就部署，无准备就开通。"
tag: reflection
---

## 第一大来源是不理解的情况下就部署了 IPv6
**IPv6 安全隐患的第一大来源，是无计划就部署，无准备就开通。**

在基本的安全配置没有的情况下匆匆上马，调通后也没有及时补救，部署安全策略，这是极其危险的做法。

## 为什么这么说？
现在的操作系统不仅早已支持 IPv6，而且在有条件的时候会优先使用 IPv6 而不是网管期望/配置的 IPv4。

这样，在 IPv6 安全策略尚未部署之时，桌面电脑和移动设备等客户端和服务器城门大开，网管自己还不知道。

## 怎么就成了安全隐患呢？
### 网络层
所有支持 IPv6 的路由器，在不明确抑制 RA（路由器宣告）的情况下，如果接口上配置了 /64 的 IPv6 全球单播地址（GUA）或本地唯一地址（ULA），就会周期性地以多播/组播形式发送 RA 报文，同时以 RA 响应主机的 RS（路由器请求）报文。

RA 信息中附带可用的 IPv6 前缀，主机就利用这个前缀生成自己的 IPv6 地址。加上主机使用 link-local 地址连接路由器，几乎不需要其他配置，主机即可访问内部外部的 IPv6 资源。

而此时，未配置的内部和边界防火墙很可能对 IPv6 流量要么置之不理（透明模式），要么像一个老老实实的路由器那样转发 IPv6 包（路由模式），内部的流量出得去，外面的流量也可以长驱直入到达客户端和服务器。

### DNS 和应用层
以上只是网络层本身的风险，实际应用中，客户端不可能不使用 DNS 来访问服务，而 DNS 的解析器和 DNS 服务器默认配置下早已支持 IPv6 资源的 AAAA 记录。

同一个域名的 AAAA 请求和 A 记录请求几乎同时发出，而客户端程序获得这两种地址时，IPv6 的应用层请求会先于 IPv4 的请求发出，间隔几百毫秒，力求用户无感知（RFC 8305 Happy Eyeballs V2），IPv4 实际成了 fallback 的手段。

假如网络的安全策略只针对站点的 IPv4 地址做了限制，那现在这种限制对用户是无效的。

### 所有双栈环境都有风险，例如远程访问 VPN
仅仅是以太网介质存在这种风险么？错，一切双栈环境都有这种风险，以上提到的情况，即整个网络环境受企事业单位自己管理，在配置了 IPv6 的 情况下出现问题，此种安全隐患还不算隐蔽。

举一个较为隐蔽的例子，这个例子是由于没有配置 IPv6：

[RFC 7359](https://tools.ietf.org/html/rfc7359) 提到了一个不容易想到的情况——远程访问 VPN 在客户端环境有原生 IPv6 时对流量失去控制。

远程访问 VPN 客户端，可以操纵本地路由表，重定向所有或是指定目的地的流量到隧道。

然而由于有些 VPN 客户端软件不支持 IPv6 或是未配置 IPv6 的 split tunnel，结果等于它们无视 IPv6 路由表的存在，任由应该流经隧道的流量利用本地 IPv6 到达互联网，造成信息安全事件。

笔者的经验里，Cisco AnyConnect VPN 客户端支持 IPv6 流经隧道，也可以给客户端下发 IPv6 地址和路由，并会时刻监视 IPv4 和 IPv6 的路由表，保证路由条目按照管理员的配置流经隧道或是直接访问互联网。

### 给 VPN 管理员的话
请各位企事业单位远程访问 VPN 服务的管理员在双栈环境下对自己使用的客户端进行测试。
* 如果不能限制 IPv6 的流量，请查阅手册修改配置以限制 IPv6 流量。
* 如果升级 VPN 系统（包括客户端）也不能解决，请发邮件给我（yinchuan@ipv6-cn.com）告知 VPN 厂商、型号、版本号，我会收集整理并在合适的时候发布，帮助其他用户和网管避开这些不安全的落后的 VPN 系统。
* 不能升级 VPN 还不能换另一家的硬件和服务么？

## 总结
今天就先谈这一些，我鼓励大家尽早开始部署 IPv6，但不希望大家在无意中打开 IPv6。

部署前先制定粗略的部署规划（即使是小范围实验），务必包括基本的安全配置。能想到的方面都规划到了再逐步开始部署，保护自己的网络和信息安全。

前人已经踩过了坑总结了经验，我们向他们致敬，并且保证自己不掉坑里去。

自己掌握理解网络的架构、设备的配置，比多贵的设备和服务都更有价值。

毕竟是你自己的网络和用户，没有第三方会一直对你的网络安全负责的。
