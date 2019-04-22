---
title: "Win10内置Linux子系统初体验"
date: 2017-08-29
lastmod: 2017-08-29
draft: false
categories: ["随笔"]
author: "sherry"
---
# 前言

主力系统之前一直用着Win7，最近也开始感觉有点审美疲劳。一直考虑是换成Win10还是Ubuntu，考虑再三还是选择了Win10...

体验了Win10半个月后，感觉系统过于臃肿。今天忽然发现Win10还有个beta项目————适用于Linux的Windows子系统。后来查了下才知道早在今年三月份就有了...(lll￢ω￢)既然如此，那果断要来体验一把！

## 教程

根据官方的[Guide](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)，首先要查看系统的OS版本号和系统类型。系统类型必须是64位操作系统以及OS版本号要在1607+.

<!--more-->

查看方法：
> 设置->系统->关于

### 安装

之后需要安装适用于Linux的windows子系统。

1.以管理员运行Windows PowerShell,输入以下内容回车
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

2.重新启动Windows

之后打开开发者人员模式。方法如下：
> 设置->更新和安全->针对开发人员

开启后，打开 命令提示符，输入 bash，会提示下载Ubuntu，输入 Y 回车即可。

提示：如果国内下载速度过慢或根本无法下载，需要自备梯子。

### 卸载

打开命令提示符，输入 lxrun /uninstall /full，然后根据提示输入 Y 即可开始卸载。

## 体验结论

这个Windows内置的Linux就是将Linux的系统调用实时的转换成Windows的系统调用，就像是Linux上的Wine的反向技术。Wine可以将Windows上的应用放到Linux上运行，这势必会降低微软的市场占有率，感到危险，微软有所动作也是理所当然。wine开源，微软也毫无办法。

不过Windows内置的Linux感觉并不自由，内置的文件不能用Windows的工具来创建或修改。否则可能会出现文件损坏导致Linux系统无法使用。因此，如果是Linux的初学者，用这个还不错，但是要当作开发环境，那就算了吧。

> 官方安装指南：https://msdn.microsoft.com/en-us/commandline/wsl/install_guide
>
> 详情链接：https://blogs.msdn.microsoft.com/commandline/2016/11/17/do-not-change-linux-files-using-windows-apps-and-tools/