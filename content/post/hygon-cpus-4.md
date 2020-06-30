---
title: "「译文」| 基准测试：Windows(四)" 
date: 2020-06-16
lastmod: 2020-06-16
draft: false
categories: ["To The New World"]
tags: ["测试中国的 x86 CPU"]
author: "sherry"
---
对于这两个系统，我们安装了 Windows：小型的8核 Dhyana 系统装了 Windows 10 专业版，而大型双32核 Dhyana Plus 服务器上安装 Windows 10 企业版，由于 AVX / AVX2 无法正常工作，因此我们的测试范围受到了限制。正如之前所说的，某些软件在系统上无法运行，例如服务器上的 CPU-Z。

<!--more-->

![img](https://s1.ax1x.com/2020/06/16/NFBZL9.png)

![img](https://s1.ax1x.com/2020/06/16/NFBVsJ.png)

![img](https://s1.ax1x.com/2020/06/16/NFBmZR.png)

![img](https://s1.ax1x.com/2020/06/16/NFBiGT.png)

![img](https://s1.ax1x.com/2020/06/16/NFBFRU.png)

![img](https://s1.ax1x.com/2020/06/16/NFBkzF.png)

![img](https://s1.ax1x.com/2020/06/16/NFBEM4.png)

从数据中可以看出，由于时钟速度的原因，8核 Dhyana 处理器介于6核 Ryzen 5 1600X 和8核 Ryzen 6 1800X 之间，但在某些方面的测试中，它甚至还不如速龙(Athlon) 200GE。而双32核 Dhyana Plus 服务器的结果似乎看起来杂乱无章，很多方面甚至被 Ryzen 7 1800X 吊打，即使有些方面能够超过 Ryzen 7 1800X 但却轻易的被 Ryzen 9 3950X 吊打。唯一一个确实表现出色的基准测试是 Corona——一个基于内存/[NUMA(Non-uniform memory access)](https://en.wikipedia.org/wiki/Non-uniform_memory_access) 的不可知整数渲染器——这一点的测试结果看起来倒是和这个芯片很相合。

(未完待续...)

## 其他篇章

1. [「译文」| 测试中国的 x86 CPU：深入研究基于 Zen 的 Hygon Dhyana 处理器(一)](https://sherry.ml/snow/post/hygon-cpus-1/)
2. [「译文」| 我们的海光系统：8核 Dhyana 和双32核 Dhyana Plus(二)](https://sherry.ml/snow/post/hygon-cpus-2/)
3. [「译文」| 海光 CPU：中版的加密方式，不同的性能(三)](https://sherry.ml/snow/post/hygon-cpus-3/)
4. [「译文」| 结论(五)](https://sherry.ml/snow/post/hygon-cpus-5/)