---
title: 论文笔记：GPU-Ether
summary: 本文为论文笔记

authors:

date: ""
publishDate: "2021-12-14T17:17:00+08:00"
lastmod: ""

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

comments: true
profile: true
share: false

gitment: true
slug: GPU-Ether-cn

featured: false
headless: false
draft: true
private: false

tags:
- 加速器互连

categories:
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

AI应用越来越广泛，其对算力的需求越来越高，因此GPU之间的高效互连是一个很重要的研究方向。以往对GPU的通信优化都需要专用网卡硬件的支持，例如HCA、SNIC或者FPGA等，要在大规模数据中心实现这些并不现实，本文实现了一种仅在软件层面进行修改从而让GPU能通过P2P直接与商用以太网卡进行连接的方法，从而降低延迟和CPU占用率。

要实现这一方法需要面对两个问题：1）GPU的线程以warp为单位进行调度，同一个warp内的线程同时执行同样的指令，而warp之间的调度是顺序随机的；2）GPU没有线程锁机制，不同的线程在申请同一块缓冲区的时候可能会冲突。

## **以太网卡工作过程**