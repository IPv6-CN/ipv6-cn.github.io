---
layout: post
title: "IPv6 地址规划与分配——全球可路由地址"
categories: [addressing]
tags: tutorial
---

## 科学规划 IPv6 地址空间的意义：
* 增强网络可管理能力，因为科学规划的网络地址——
  * 能正确反映网络拓扑结构
  * 能减小网络配置和维护过程中的人为失误
  * 使得安全策略制定更为容易
  * 降低了故障排除时的工作难度
  * 能为未来扩容预留空间，少量子网的增加不需要大规模架构和安全策略调整

* 提高网络可扩展性，关键在抑制路由表无序增长——
  * 较小的路由表节约了设备硬件资源，同样的设备能支撑更大的网络
  * 较小的路由表提高了网络结构变化时路由收敛的速度
  * 要使路由聚合变得容易，必须在二进制边界划分

## 几种常见前缀长度和分配场景

|前缀长度|适用场景|
|---|---|
|32|RIR/NIR（区域/国家互联网注册机构）分配给有 ASN 的运营商、互联网公司、大型企业。是地址最小分配单元（再小就不给了）。|
|40|运营商向有多个（256个以内）站点和数据中心的大型企业分配的前缀|
|44|运营商向有多个（16个以内）站点和数据中心的中型企业分配的前缀|
|48|运营商向中小客户分配的常见前缀长度。或大中企业内一个站点的前缀|
|56|宽带运营商给家庭用户和小微企业分配的最小前缀长度（最大子网大小）|
|64|末端设备子网，/64 是很多协议硬性要求的（IPv6 无广播风暴风险）|
|127|路由器点对点链路，此处不是为了节约地址而是防止一种资源耗尽型攻击|

由于 IPv6 的接口地址部分，即后 64 位，所能容纳的地址数量远超过现有任何设备的硬件转发表项，可以近似看做无限的地址空间，完全不需要考虑节约地址的事情。

客户端接入的二层网络的规模（即每个 /64 所对应的 VLAN）要尽量小，因为二层网络的状态难以使用成熟的分布式路由协议进行分发，网络规模发展会很快遭遇瓶颈。笔者目前的推荐是一个 VLAN 不要超过 2012 个有唯一 MAC 地址的网络接口。

虽然在路由层面，设备都支持任意长度的前缀，但为了使维护人员无需在紧急排障时还要计算子网范围，请务必在 4 比特边界，即每个 16 进制数处划分子网。

如果根据计算，获得的前缀不足以支撑足够多的 /64 子网，请尽快向地址分配机构请求（空间）更大的地址前缀，即更短的前缀。

本文中使用的所有全球单播地址前缀均为标准文档专用地址：`2001:db8::/32`

几个不同前缀长度地址的例子和所包括的地址范围：

|前缀长度|示例地址|地址范围|
|---|---|---|
|32|`2001:db8::/32`|`2001:0db8:0000:0000:0000:0000:0000:0000`<br/>`2001:0db8:ffff:ffff:ffff:ffff:ffff:ffff`|
|40|`2001:db8:ab00::/40`|`2001:0db8:ab00:0000:0000:0000:0000:0000`<br/>`2001:0db8:abff:ffff:ffff:ffff:ffff:ffff`|
|48|`2001:db8:abcd::/48`|`2001:0db8:abcd:0000:0000:0000:0000:0000`<br/>`2001:0db8:abcd:ffff:ffff:ffff:ffff:ffff`|
|56|`2001:db8:abcd:1200::/56`|`2001:0db8:abcd:1200:0000:0000:0000:0000`<br/>`2001:0db8:abcd:12ff:ffff:ffff:ffff:ffff`|
|64|`2001:db8:abcd:1234::/64`|`2001:0db8:abcd:1234:0000:0000:0000:0000`<br/>`2001:0db8:abcd:1234:ffff:ffff:ffff:ffff`|

### 那属于运营商和自己的地址在配置时有何不同？
除了使用自己的地址在边界路由器上需要运行 BGP 与上游运营商建立关系以外，内部地址的划分没有不同。

如果预计要经常切换运营商，则必须考虑申请自己的 ASN 和 IPv6 地址前缀，与上游运营商使用 BGP 交换路由信息。

### 给宽带运营商的话：
* 根据 APNIC 的这份《IPv6 地址分配方针》：http://www.apnic.net/ipv6-guidelines ，请给家庭宽带用户分配 /56 的前缀（256 个大小为 /64 的子网）。使用 DHCPv6 PD 前缀下发协议 RFC 3633 或者 IPv6CP 和 DHCPv6 PD 的组合（如果是 PPPoE 连接）。
* 给企业分配前缀时，请考虑其未来几年的发展规模，为增长预留空间，比如：分配一个 /48 前缀的同时，为其预留一个包含此 /48 的 /44 的前缀。而不是将接下来的 /48 分配给下一个企业客户。这里 /44 只是一个推荐值，为规模不会增长过大的组织预留一个 /46 也是可行的。若给甲单位分配了 `2001:db8:5430::/48`，那 `2001:db8:5430::/48` 到 `2001:db8:543f::/48`，包含这 16 个 /48 的 /44 的空间都应为其预留。乙单位的前缀应从 `2001:db8:5440::/44` 中分配


## 示例：两种常见情况下的地址分配

### 获得运营商静态分配 /48 前缀的单位和企业
1. 这个前缀不适合有多个大型园区，每个园区超过 16 个楼宇或数据中心的场景。但是对于单个大园区、或多个小园区是合适的。
2. 考察现有 IPv4 子网分配情况，假设内部没有额外一层受企业控制的 NAT（即没有用户私接 NAT 路由器之外的情况），统计子网个数。如果超过 65536 个，检查有无因为某个子网地址不够分配而新加子网的情况。
3. 一个 /48 可以划分为 16 个 /52，256 个 /56，4096 个 /60，65536 个 /64 大小的子网。每一位分配时，建议预留 0000 (0) 和 1111(F)，避免书写和阅读障碍。
4. 前两位 16 进制数的划分可以有以下两种思路，笔者推荐第一种。

#### 先考虑路由聚合，再考虑基于地址的安全策略。

##### 第 49 – 52 比特，可能的取值是 0 – F 的十六进制，去掉头尾共 14 个数。可以给每个楼或者小型园区分配一位。对网络核心方向只发布属于自己的一条聚合路由。

|功能块/地理位置|/52 长度地址前缀|
|---|---|
|办公楼|`2001:db8:abcd:1000::/52`|
|教学楼A|`2001:db8:abcd:2000::/52`|
|教学楼B|`2001:db8:abcd:3000::/52`|
|宿舍楼A|`2001:db8:abcd:8000::/52`|
|数据中心A|`2001:db8:abcd:c000::/52`|
|数据中心B|`2001:db8:abcd:d000::/52`|

  * 办公区可以聚合为 `2001:db8:abcd:0000::/50`<br/>宿舍区可以聚合为 `2001:db8:abcd:8000::/50`<br/>数据中心可以聚合为 `2001:db8:abcd:c000::/50`

##### 第 53 – 56 比特，共 14 个可能的组合，可以给每个安全组分配一位。以上面的办公楼为例：

|安全组|/56 地址前缀|
|---|---|
|网络设备|`2001:db8:abcd:1100::/56`|
|楼内服务器|`2001:db8:abcd:1200::/56`|
|安防系统|`2001:db8:abcd:1300::/56`|
|会议系统|`2001:db8:abcd:1400::/56`|
|IP 电话|`2001:db8:abcd:1500::/56`|
|网络信息化中心|`2001:db8:abcd:1800::/56`|
|办公室无线|`2001:db8:abcd:1c00::/56`|
|办公室有线|`2001:db8:abcd:1d00::/56`|
|学生无线|`2001:db8:abcd:1e00::/56`|

合理分配安全组前缀，简化 ACL 条目数量。

这样分配后，网络内路由表条目达到最少，如果每个 /52  子网一样的话安全策略条目大部分都是简单的重复，可以批量生成。

#### 先考虑安全策略，再考虑路由聚合。

* 第 49 – 52 比特，给每个安全组分配一个。
* 第 53 – 56 比特，给每个楼或者小型园区分配一位。

这样所有设备上的安全策略可以达到一致，大大降低了安全相关的维护成本。

路由条目较多，不过对于现代的动态内部路由协议（IGP）例如 OSPF，EIGRP 和 IS-IS 也不是问题。但需要更多路由聚合的配置分散在各种网络边界设备上，才能控制住路由表规模。

随着现在诸如 BeyondCorp（来自 Google） 的理念普及，用户的访问权限不再与其所在子网和网络地址相关，只在应用层对用户进行认证和鉴权。甚至可以省略掉这一位作为安全边界的划分，为内部提供更多可用子网。

#### 上面分析了 49-56 比特的地址分配，接下来讨论剩下的部分
* 后两位十六进制数——第 57 – 64 比特，可以分为 256 个子网，去掉 00 和 FF，还要 254 个，可以用在楼宇内部 VLAN 划分。规划时可以与现有 IPv4 地址的第三部分共用，还可以与 VLAN ID 对应，总之，为了便于识别和故障排除。

   建议使用这种方法部署时，给每个人发一张 0 – 255 十进制和十六进制对照表。

* 第 65 – 128 比特，使用 SLAAC 无状态分配或 DHCPv6 有状态分配。

如果有多个不同地理位置的站点和数据中心，先按照地理位置分配，再考虑安全和流量策略分配。

所得前缀长度小于 /48 的可以参考此划分方法，为未来发展进一步预留空间，比如配置 /56，预留包含此 /56 的 /52。

### 获得运营商静态或动态分配 /56 前缀的家庭
先来帮运营商算一下：

* 一个 /44 的前缀，可分配 4096 个 /56。笔者所在小区算下来约 3780 户，完全容纳还有空间。
* 一个 /32 的前缀，可分配 4096 个 /44，即使个别小区较大，需要比 /44 大的网络，也能支持四千个左右小区，共 16777216 户，1677.7 万户。
* 一个 /20 的前缀，最终可以分配约 687.2 亿户

所以请务必**不要节约**前缀，而是从未来网络发展的角度来考虑，为家庭用户分配 /56 的前缀。

与企业网络层次结构不同，家庭网络的前缀分配是扁平化的，通常所有设备都连接在同一个路由器上，重新编址也不会太难。

在目前应用尚不丰富的情况下，我们从 `2001:db8:1234:4300::/56` 中选择一个 /60 前缀 `2001:db8:1234:4320::/60` 来为家庭网络分配地址。

一个 /60 前缀包含 16 个 /64 前缀，保留第 61 – 64 位全 0 和全 1 的，不使用。可以直接考虑安全策略为主的划分。

* 0 – 3 内部网络服务
  * 0 保留
  * 1 网络基础名称和认证服务
  * 2 应用系统
  * 3 存储备份
* 4 – 7 终端接入
  * 4 客人无线
  * 5 主人无线
  * 6 主人有线
  * 7 联网打印机等办公设备
* 8 – B 物联网设备
  * 8 各类控制器
  * 9 监控
  * A 物联网预留
  * B 物联网预留
* C – F 未来发展预留
  * F 保留

## 总结

分配前缀时要把以下几个重要思想牢记：
1. 地址合理分配后，组织进一步扩大规模是不需要对网络进行大规模改动或重编址的。
2. 网络局部发生变化时，细节信息不应当传播到不关心的区域（路由设计问题）。
3. 用户应当有条件获得较大子网以进行网络隔离。
4. 组织的网络拓扑和用户与地址的对应关系要控制在组织内部，对外有保护措施。
5. 二层网络会制约网络健康发展，要有意识地利用三层路由来控制二层网络规模。
6. 路由表尽量小，让网络管理员看起来每一位都有意义。
7. 重复性操作要考虑使用脚本完成。
8. 注意由于地址空间扩大带来的有意无意的拒绝服务攻击风险。

IPv6 前缀的分配一定要考虑未来五年十年的发展需求，地址重编码并不简单。虽然 IPv6 支持多个前缀同时使用，但客户端行为目前还不确定，要无缝切换需要更多精力周密规划，尤其是网络结构调整造成的地址重编码。

如果你遇到的场景不能直接套用前面的例子，请邮件与我联系 yinchuan@ipv6-cn.com ，我会考虑增加更多实例。

**任何使用本文制作出版物的行为需经本人书面同意，并支付稿酬。**

## 参考资料（英文原文）
* [RFC 3633](https://tools.ietf.org/html/rfc3633)——DHCPv6 中的 IPv6 前缀选项
* [RFC 4192](https://tools.ietf.org/html/rfc4192)——不设定割接日期，为一个 IPv6 网络重新分配地址的步骤
* [RFC 4241](https://tools.ietf.org/html/rfc4241)——一种 IPv6/IPv4 双栈互联网接入层模型
* [RFC 6177](https://tools.ietf.org/html/rfc6177)——终端站点 IPv6 地址分配（当前最佳实践）
* [RFC 6879](https://tools.ietf.org/html/rfc6879)——IPv6 企业网地址重新分配的场景、注意事项和方法
* [APNIC guidelines for IPv6 allocation and assignment requests](http://www.apnic.net/ipv6-guidelines)——亚太地区网络信息中心 IPv6 地址分配方针
* [RIPE Preparing an IPv6 Addressing Plan](https://labs.ripe.net/Members/steffann/preparing-an-ipv6-addressing-plan)——准备一个 IPv6 编址计划
