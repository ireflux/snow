---
title: "Linux 通过虚拟机路由转发连接 EasyConnect"
date: 2022-01-19
lastmod: 2022-01-23
draft: false
categories: ["Clavicula Salomonis"]
tags: ["linux", "virtualbox", "easyconnect"]
author: "sherry"
---
EasyConnect 虽然也有 Linux 版本，但在网上逛了一圈，好像会有一些坑，而且需要 root 权限执行一些操作，对于一向没啥节操的国产软件还是果断放弃物理机直接装的方案。但我还想在我的 Arch 上连内网，本着某些国产软件能不装到物理机就不装到物理机的原则，就想通过连着 EasyConnect 的虚拟机来上内网，又在网络上搜寻了一圈，没想到还真的找到一些解决方案，果然大家的痛点都是一样的。

<!--more-->

### 虚拟机设置

首先在虚拟机上开启两个网卡，我的是 VirtualBox，就以它为例，依次进入 `设置->网络`，勾选两个网卡，启用网络连接，网卡一用于连接外部网络，连接方式 NAT 或桥接都可。网卡二用于和 Linux 物理机交换数据，连接方式选择 “[仅主机（Host-Only）网络](https://www.virtualbox.org/manual/ch06.html#network_hostonly)”。顺便一提，如果选择完 “仅主机（Host-Only）” 后，界面名称选项为空的话，就需要去菜单 `管理->主机网络管理器` 中创建一个，然后再回到原来的地方，界面名称的选项就是刚刚在网络管理器中创建的。

打开 Windows 虚拟机后，依次进入 `控制面板->网络和共享中心->更改适配器设置`，里面会存在 “Sangfor SSL VPN CS Support System VNIC” 的本地连接和两个其他本地连接，其中一个是 “NAT/桥接” 的网卡一，另一个是 “仅主机（Host-Only）” 的网卡二，需要自己分辨一下。在 “Sangfor SSL VPN CS Support System VNIC” 的本地连接上依次 `右键->属性->共享`，勾选 “允许其他网络用户通过此计算机 Internet 连接来连接” 选项，家庭网络连接选择 “仅主机（Host-Only）” 的本地连接，确定即可。查看一下 “仅主机（Host-Only）” 的 IP 地址，接下来需要在宿主机上设置路由，将要内网地址转发到 “仅主机（Host-Only）” 的 IP 地址上。

### 宿主机设置

以我的为例，我的 “仅主机（Host-Only）” 的本地连接 IP 地址为 192.168.56.3，假设我要访问的公司内网 IP 在 172.16.x.x 的网段，就可以将 172.16 开头的内网地址全部转发到 “仅主机（Host-Only）” 的 IP 地址上，即在宿主机执行：

```bash
$ sudo route add -net 172.16.0.0/16 gw 192.168.56.3
```

命令执行完后可以通过 `route` 查看一下：

```bash
$ route
```
| Destination | Gateway | Genmask | Flags | Metric | Ref | Use | Iface |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| default | _gateway | 0.0.0.0 | UG | 600 | 0 | 0 | wlp3s0 |
| 172.16.0.0 | 192.168.56.3 | 255.255.0.0 | UG | 0 | 0 | 0 | vboxnet0 |
| 192.168.31.0 | 0.0.0.0 | 255.255.255.0 | U | 600 | 0 | 0 | wlp3s0 |
| 192.168.56.0 | 0.0.0.0 | 255.255.255.0 | U | 0 | 0 | 0 | vboxnet0 |

至此就可以在宿主机上愉快的上内网了。

刚刚添加的路由转发规则是临时的，宿主机重启后，就会重置。也可以在用完后手动删除，即在终端执行：

```bash
$ sudo route del -net 172.16.0.0/16 gw 192.168.56.3
```

## 后记

这么做虽然繁琐一点，但可以使得我的 Arch 连上内网的同时又避免了直接在物理机上装软件，还是感觉极度舒适。

## 参考资料

1. [Chapter 6. Virtual Networking](https://www.virtualbox.org/manual/ch06.html): https://www.virtualbox.org/manual/ch06.html