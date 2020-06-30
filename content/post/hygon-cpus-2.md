---
title: "「译文」| 我们的海光系统：8核 Dhyana 和双32核 Dhyana Plus(二)" 
date: 2020-06-07
lastmod: 2020-06-07
draft: false
categories: ["To The New World"]
tags: ["测试中国的 x86 CPU"]
author: "sherry"
---
海光的所有已知发行版可归结为两个平台：一个包含一个8核 Zen 1 芯片，类似于台式机系列处理器（或 EPYC 3000），以及一组由四个芯片构建的服务器处理器，布局和 Naples 系列相似。

<!--more-->

![img](https://s1.ax1x.com/2020/06/07/t2jV9P.jpg)

我们要测试的8核工程样本系统没有提供任何准确的 SKU 编号——CPU上标着“C86”，代表“中国的 x86”。其余的编号可能与硅片所来自的晶圆和批次相关，但是我们没有可用的[解码器环](https://en.wikipedia.org/wiki/Secret_decoder_ring)。

![img](https://s1.ax1x.com/2020/06/07/t2jktI.jpg)

主板使用 [microATX](https://en.wikipedia.org/wiki/MicroATX) 尺寸，并且是一个带有横向 DDR4 插槽的服务器主板，以便引导气流通过服务器机箱中的系统。现在从最主要的插槽开始说，由于我们目前拿到的 CPU 是无插槽 BGA 设计，已经绑定到主板上了，因此无法升级，这个有点类似于我们在笔记本电脑和嵌入式系统中看到的那种。而且这些安装孔让我感到有点惊讶——这些不是我们通常在 Ryzen 消费级产品中看到的那种 AM4 安装孔，而是 Intel 的安装孔。我猜肯定是有人有很多旧的 Intel 散热器？要么就是为了能够更容易找到特定的服务器级的散热器。

这个 CPU 具有六相电力输送系统，并且由于这个是纯 CPU，因此没有集成图形。左下方是 IPMI 控制器提供的 2D 图形，这是我们在服务器系统中常见的经典的[信驊科技](https://www.aspeedtech.com/)的 AST2500 芯片。不像其他 microATX 主板，这个没有典型的四插槽设计，而是三插槽设计，带有两个全长 PCIe 3.0 插槽（能够支持 x16/x0 或 x8/x8）和一个开放式 PCIe x4 插槽。

通常，这些 CPU 确实不需要芯片组，因为它们在芯片上具有 SoC 级别的 IO 功能，尽管在 Ryzen 1000 家族首次推出时，消费级的 Ryzen CPU 需要与 X370 芯片组搭配使用。但海光并未使用这些芯片组，而是将 CPU 与 [Lattice 半导体公司](https://www.latticesemi.com/)的现场可编程门阵列(FPGA)搭配作为芯片组。于是从整体上来看这个主板提供了一套奇特的组合 IO，包括 SATA 端口，四个双 LED 显示屏，许多自定义的连接器和按钮以及许多我们不知道的未记载的东西。例如，主板上似乎有两块电池——一个应该是用来保持开机时间，另一个看起来似乎时间更久但是我们不知道为什么要放在那个地方。

![img](https://s1.ax1x.com/2020/06/07/t2jZ1f.jpg)

这是主板，我们在上面放置了等效的 AM4 消费级 Ryzen CPU，以显示尺寸。

![img](https://s1.ax1x.com/2020/06/07/t2jAht.jpg)

相比之下，双插槽服务器有点像一头野兽。据我们了解，这些服务器是为计算和存储而构建的，每个 CPU 都与四个具有4个 U.2 驱动器或 16 路 SATA 连接能力的分支连接器配对。 CPU 具有八通道内存功能，但是由于一些原因，我们不得不在四通道模式下对其进行测试。

![img](https://s1.ax1x.com/2020/06/07/t2jFAA.jpg)

这个 CPU 虽称为 C86，但型号上也标明着数字7185，表示是 32 核 CPU。外壳是红色的，而 Naples EPYC CPU 为蓝色，Threadripper CPU 为橙色，Rome EPYC CPU 为绿色。红色可能是对中国的致敬，但也没人能和我们确认这一点。

![img](https://s1.ax1x.com/2020/06/07/t2jec8.jpg)

该服务器实际上是曙光信息产业有限公司设计，具有12个前面板2.5英寸驱动器插槽。对于这个8核系统，我们将其放入标准台式机壳中并配备 CPU 散热器。两种系统都通过远程桌面访问进行了测试，因为当我回到伦敦时，它们已经被 Wendell 托管在他在肯塔基州的实验室中了。

![img](https://s1.ax1x.com/2020/06/07/t2vp80.jpg)

当我们尝试探测该 CPU 时，CPU-Z 似乎没有太多线索。该软件为8核用户提供了该接口，显示了 3.2 GHz 频率，但只有一个核，除了对 AVX，AVX2 和 FMA3 的支持外，没有其他的细节。对于服务器 CPU，CPU-Z 完全无法运行。而且似乎对于海光的型号，在 AMD 消费级 CPU 上访问数据的一些常用方法都已经全部更改，要么规避常规方法检测，要么遵循不同的标准。有趣的一点是，尽管 CPU-Z 检测到 AVX 和 AVX2，但我们的某些软件无法检测到，因此我们不得不恢复为 SSE 检测，以使该软件运行。

(未完待续...)

## 其他篇章

1. [「译文」| 测试中国的 x86 CPU：深入研究基于 Zen 的 Hygon Dhyana 处理器(一)](https://sherry.ml/snow/post/hygon-cpus-1/)
2. [「译文」| 海光 CPU：中国版的加密方式，不同的性能(三)](https://sherry.ml/snow/post/hygon-cpus-3/)
3. [「译文」| 基准测试：Windows(四)](https://sherry.ml/snow/post/hygon-cpus-4/)
4. [「译文」| 结论(五)](https://sherry.ml/snow/post/hygon-cpus-5/)