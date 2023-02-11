---
title: "ArchLinux安装教程"
date: 2017-10-18
lastmod: 2017-10-18
draft: false
categories: ["随笔"]
tags: ["Linux"]
author: "sherry"
---
体验了 Win10 一个多月后，感觉不光系统臃肿，还有些迷之 BUG，首先是点关机后，系统实际上并未关闭，根据网上的方法禁用了 intel management engine interface 驱动后解决。另一个是看视频暂停后，再继续声音会突然变大，只能在系统音量那随便调一下就正常。虽然并不怎么影响使用，但作为完美主义者这是绝对无法忍受的。曾数次 Google，无论是禁用声音效果还是改声卡驱动都无济于事……

心灰意懒，于是在使用了 Win10 一个多月后，我又决定要换系统了:D，这大概是我使用 Windows 系统以来时间最短的一个版本了吧（笑。

<!--more-->

入了 Arch 神教有段时间了，发现这样的系统对我这种日常需求比较少的来说正合适。而且滚动更新很适合像我这种喜欢体验新版本的人。当时安装时并没有看官方 Wiki，因为中途需要点各种链接跳来跳去，刚装 Arch 的人都想按照一个教程一步步走下来把它装好，有时候看到别人说的装 Arch 时走了多少坑，突然发现有个好教程是多么幸运的一件事。当时看的另一位 dalao 的基于官方 Wiki 的中文教程，遂引用如下：

1. [以官方 Wiki 的方式安装 ArchLinux](http://www.viseator.com/2017/05/17/arch_install/)  
2. [ArchLinux 安装后的必须配置与图形界面安装教程](http://www.viseator.com/2017/05/19/arch_setup/)

在这里说一句，第二篇设置 NetworkManager 开机启动时，如果提示错误，那就是还没装 NetworkManager，先安装下一步的 network-manager-applet，它会把 NetworkManager 作为依赖项装上，然后再设置开机启动。

至此，enjoy！