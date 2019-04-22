---
title: "Linux下MPlayer编译教程"
date: 2017-11-8T10:14:00+8:00
lastmod: 2017-11-8T10:14:00+8:00
draft: false
categories: ["随笔"]
author: "sherry"
---
# 前言

本教程基本来自官方文档，我自己也权当做个记录(虽然之后我也换了VLC)，如果喜欢定制的话，选这个还是没错的.建议有基础的还是去看官方.官方建议自己编译，不建议从第三方库中直接安装(虽然直接装也没问题)。本教程只讨论Linux平台下，Windows如无特殊需求，不需要Mplayer(估计PotPlayer一统江湖吧)。本文若有错误疏漏之处，还请告知。

## 编译之前

编译之前需要准备一些东西，Source Code,Binary Codec Packages,Skins。这些都可以在[官方网站](http://www.mplayerhq.hu/design7/dload.html)获得.

Source Code没什么好说的，选择最新的就行。Binary Codec Packages，需要根据本机情况选择。

> 如果是x86，就选择Linux x86
>
> x86_64就选择Linux AMD64

<!--more-->

skins随便选一个就好。没什么要推荐的：D。把这些东西随便放在你想放置的地方。分别解压缩，然后，就准备开始吧！

提示：如无特殊说明，下面一切操作皆在MPlayer目录下进行。

## 安装Binary Codec Packages

把Binary Codec Packages放到Mplayer能找到的地方，默认的目录是/usr/local/lib/codecs/

## 配置MPlayer

首先Mplayer会检测所需的硬件环境，在终端中运行
> ./configure

来使用默认的选项配置MPlayer。如果想要图形界面，需要运行
> ./configure --enable-gui

如果有些东西不能用，尝试输入
> ./configure --help

来查看可用的选项并选择你想要的功能。

## 编译MPlayer

如果上面一切顺利，下面就可以开始编译了。在终端中输入：
> make

安装MPlayer
> make install

然后就等着就行了。

## 安装图形界面的皮肤

将你之前解压完的skins得到的文件夹改名为default，并移动到/usr/local/share/mplayer/skins/目录中。MPlayer将会自动启用该皮肤。

## 结尾

一切顺利的话，它会在菜单中创建一个MPlayer，你可以直接双击视频来打开，或者在终端输入`mplayer <moviefile>`或`gmplayer <moviefile>`来打开一个视频。

至此，MPlayer就安装完毕了，Enjoy!