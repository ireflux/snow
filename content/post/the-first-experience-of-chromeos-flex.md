---
title: "ChromeOS Flex 初体验" 
date: 2022-07-20
lastmod: 2022-07-20
draft: false
categories: ["随笔"]
author: "sherry"
---
最近看到了 Google 推出了他们操作系统「ChromeOS Flex」，声称适用于任何场合。我记得之前就看到过 Google 新操作系统的消息，好像之前还只是在测试阶段，但也允许下载体验。由于是非正式版本就没有尝试，不过最近终于看到他们释出正式版本，而且可以直接装到 USB 上启动，那得好好体验一下。

<!--more-->

## 安装

按照官方的[教程](https://support.google.com/chromeosflex/answer/11552529?_ga=2.40629794.1417789089.1658235257-1827319735.1658235256)先准备一个非闪迪的 U 盘（因为闪迪会有一些[已知的问题]((https://support.google.com/chromeosflex/answer/11541806#usb))），然后去 
[Chrome web store](https://chrome.google.com/webstore/detail/chromebook-recovery-utili/pocpnlppkickgojjlmhdmidojbmbodfm) 安装 `Chromebook Recovery Utility` 这个扩展（并不支持 Linux 系统)，插上 U 盘后，打开新安装好的扩展，制造商选择「Google Chrome OS Flex」，产品选择「Chrome OS Flex」，点继续之后，接下来它就会自动下载并写入到你的 U 盘（话说我还是第一次见到用浏览器扩展制作启动盘的...），期间会清空 U 盘上的所有信息。这个过程完成之后，你就拥有一个 Chrome OS Flex 的启动盘了，剩下的也无需赘述。

## 体验

说说使用体验，系统启动时画面上中央会出现 ChromeOS Flex 的字样，感觉还是挺不错的。系统刚进去会有一个使用向导，一步步设置：选择语言，设置键盘布局，连接网络，登录账号，整体 UI 和交互就是原生 Android 的既视感。进去之后自带了 Google 全家桶，其他的不知道都能安装什么第三方应用，也没有看到有类似应用商店之类的东西。设置里有提供 Linux 相关工具的入口，但需要额外安装，迫于 U 盘空间不大，这部分就没体验，打开 terminal 也提示需要额外安装那个 Linux 工具集，但自带了 SSH 连接的图形化界面。打开设置里面查看系统设置信息又给人一种整个操作系统就是一个 Chrome 浏览器的感觉...

总之体验下来个人感觉作为一个个人电脑的操作系统还是不太够，除了一个浏览器，其他的没什么可玩儿的。如果作为其他终端的操作系统，做一些定制化或许还可以，比如车载系统，银行 ATM，KTV 的点歌台之类的

## 参考资料

1. [Chrome OS Flex installation guide](https://support.google.com/chromeosflex/answer/11552529?hl=en&visit_id=637939238602881027-1929521378&ref_topic=11551271&rd=1)
2. [Get ready to flex this summer: ChromeOS Flex is now ready to scale broadly to PCs and Macs](https://cloud.google.com/blog/products/chrome-enterprise/chromeos-flex-ready-to-scale-to-pcs-and-macs)