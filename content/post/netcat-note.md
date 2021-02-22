---
title: "netcat 的使用笔记"
date: 2020-03-16
lastmod: 2020-03-16
draft: false
categories: ["Clavicula Salomonis"]
tags: ["linux", "netcat"]
author: "sherry"
---
当在[ debian 的仓库](https://packages.debian.org/sid/netcat-openbsd)中查看 openbsd 版本的 netcat时，会看到描述中有这样一句话：“TCP/IP swiss army knife”，被誉为 TCP/IP 的瑞士军刀，可以想象，这可以说是很高的赞誉了。netcat 当年还是2001年，2003年，2006年的 insecure.org 几次安全工具的投票中的前五名，它不仅是一个非常优秀的软件，体积还很小（只有几十KB），[源代码](https://github.com/openbsd/src/tree/master/usr.bin/nc) 也很少，以至于有很多人去重写，出现了很多变种。比较知名的有[openbsd版本](https://packages.debian.org/sid/netcat-openbsd)，[gnu版本](http://netcat.sourceforge.net/download.php)，[nmap社区版本ncat](https://nmap.org/ncat/)。

<!--more-->

netcat 最初的作者是一个名为“*Hobbit*”的人在1995年10月28号以源代码的形式发布的。在其[官网](https://nc110.sourceforge.io/)还能找到一些其他信息以及由“*Hobbit*”最后更新于2007年的那个版本。在本文中，我使用的是 openbsd-netcat，因此以下使用记录如无特殊说明默认都是 openbsd 版本的netcat。

首先来看一下 `nc -h` 里平时使用频率比较高的一些选项：

- h：输出帮助信息
- k：可以使客户端重复连接，一般配合 l 来使用
- l：开启监听模式，作为服务端。若不加该选项则默认为客户端
- n：不对命令行中的host进行域名解析。简单来说就是host写为x.x.x.x时加上该选项，写为域名时不加
- p：指定端口
- v：输出详细信息
- z：通常用于端口扫描，仅判断是否能够连接，不进行数据通讯

### 开启服务

```bash
$ nc -l -k -p [port]
```

作为服务端，监听端口，开启服务。假设客户端通过 `nc [ip] [port]` 连上服务端，当断开客户端，服务端也会停止，加上选项 k 可以使其保持开启状态。这个是挺常见的用法，使用频率挺高，例如在flink的官方教程就用到了这个。

### 测试端口

```bash
$ nc -nv [ip] [port]
```

例如某个远程主机上开启了某个端口，这样做可以测试端口是否能连上，若其中的 ip 的位置为域名，就不需要加 n 了。

### 端口扫描

```bash
$ nc -znv [ip] [port-port] 2>&1 | grep succeeded
```

可以用来做渗透测试。

### 文件传输

首先在接收端开启监听：

``` bash
$ nc -l -p [port] > [filename]
```

然后在发送端发送文件:

```bash
$ nc [ip] [port] < [filename]
```

这样传输文件的好处是不需要应用层，直接在传输层层面传输的，相比于通过应用层的软件之间的传输，性能上会有优势。假如从本机要传文件到开启了nat模式的虚拟机中，由于此时虚拟机对于宿主机的访问是单向的，因此可以先在虚拟机上配置一个端口转发，然后从本机 `nc localhost [port] < [filename]` 。注：传输过程中没有进度条，因此需要重新开一个 terminal，自行对比文件的大小判断是否发送完后，然后在发送端 `ctrl+c` 即可。

### 后记

除了以上的常见操作外，netcat还能配合一些其他命令做到更多高端操作，诸如端口转发，代理转发，备份系统，开启后门等等，更多操作可以参考编程随想的博文——[扫盲 netcat（网猫）的 N 种用法——从“网络诊断”到“系统入侵”](https://program-think.blogspot.com/2019/09/Netcat-Tricks.html)。值得一提的是开启后门的选项 -e 在 openbsd-netcat 被删掉了，可能是作者觉得过于危险，但是在其他很多变种中这个选项还有保留。虽然 -e 在 openbsd-netcat 中被删除了，但是还是可以通过创建命名管道的方法间接达到 -e 的作用：

```bash
$ mkfifo [name]
$ cat [name] | /bin/bash 2>&1 | nc -l -p [port] > [name]
```

这可能也是编程随想的博客中有提到但并未透露的“间接的方式”。
