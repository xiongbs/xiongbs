---
title: 从tcp看网站的加载体积限制在14kb
author: beni
date: Fri Oct 21 2022 11:06:19 GMT+0800
categories: [diary, translation]
tags: [translation]
mermaid: true
---


# [为什么你的网站限制在14kb的大小比较好](#为什么你的网站限制在14kb的大小比较好)

译文：[WHY YOUR WEBSITE SHOULD BE UNDER 14KB IN SIZE](https://endtimes.dev/why-your-website-should-be-under-14kb-in-size/)

网页的内容越小，加载越快。

14kb的大小比15kb的带下明显要快，大约要快612ms(译者待求证), 然而15kb和16kb之间的效果就比较小。（trivial：琐碎）

以上效果的原因就是因为TCP的慢启动算法，本文将介绍什么是慢**算法，它怎么工作，且你应该关注的点有哪些。但是在这之前，我们先快速过一下 TCP 基础。

## 什么是TCP

Transmission Control Protocol (TCP) 使用 Internet Protocol (IP) => TCP/IP

acknowledgement or ACK（承认, 感谢, 承认书）认可对方

具体请查阅TCP

## 什么是TCP慢启动

TCP慢启动主要是TCP服务端弄清当前单位时间内可以发送的流量包大小，客户端请求链接时，服务端是不知道客户端与服务端之间的具体带宽。

服务器端是不知道客户端当前的带宽处理能力，所以服务端刚开始少量安全的数据，通常是10个TCP包。所以就有了ACK三次握手，如果客户端成功收到，会回复ACK，表明文件已经被收到，然后服务器收到这个ACK后，这次服务器就会发送双倍的数据给客户端。这一过程会一直重复，知道包丢失或者服务器没收到ACK。这就是TCP慢启动

## 14kB 规则怎么来的呢？

大多数TCP的网站通过发送是个TCP包启动TCP慢启动
TCP包最大为1500 bytes 来自互联网标准：[ethernet](https://en.wikipedia.org/wiki/Ethernet_frame)
每个TCP包头部使用**40 bytes**，[16bytes给IP](https://en.wikipedia.org/wiki/IPv4#Packet_structure)，剩下的[24bytes给TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure)

所以每个包还剩下 1460 bytes，所以总的能传输的数据为 10 * 1460bytes，约等于14kb。

所以你可以把你的TCP网站关键部分优化为14kb，这样就可以节约TCP客户端服务端交换的次数，从而节约时间。

## 那些网络环境设备不好的人们的延迟又是多少呢？[#]
比如说卫星通信，那些住在偏僻没有信号基站或者货轮，以及机场上的WIFI。
还有那些通信不发达的人们，2G，3G，嘈杂的移动网络，比如音乐节这种很拥挤的，服务器处于高并发状态时，以及那些不稳定的链接都有可能导致丢包情况发生。

## 在知道14kb规则后，现在，你能做些什么呢
先渲染容器可见的区域，加载关键资源，css、html，压缩数据保持在14kb内。

#### 关于14kb规则的一些点

这个规则更像是经验来的规则，而不是计算机的基本规则：

* 

#### HTTP/2 and the 14kb rule

说是不起HTTP/2下不在起作用了，但是读者了解很多数据之后，依旧使用着这条规则

#### HTTP/3 and QUIC

虽然提到14kb规则不在适用，[但是依旧还在使用中](https://datatracker.ietf.org/doc/id/draft-ietf-quic-recovery-26.html)

## 延伸阅读

本文参考了以下内容：

1. [High performance browser networking by Ilya Grigorik](https://hpbn.co/)
2. [Increase HTTP Performance by Fitting In the Initial TCP Slow Start Window by Simon Hørup Eskildsen](https://sirupsen.com/napkin/problem-15)
3. [Critical Resources and the First 14 KB - A Review](https://www.tunetheweb.com/blog/critical-resources-and-the-first-14kb/)
4. [HTTP3 performance improvements](https://www.smashingmagazine.com/2021/08/http3-performance-improvements-part2/)


