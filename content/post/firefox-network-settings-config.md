---
title: "利用Firefox自带的Network Settings科学上网" 
date: 2019-05-06
lastmod: 2019-05-06
draft: false
categories: ["随笔"]
tags: ["Firefox"]
author: "sherry"
---
自从Chrome 71(没记错的话是这个版本= =)曝出追踪用户的[丑闻](https://www.valuewalk.com/2018/11/google-chrome-71-track-abusive-experiences/)后，我的主力浏览器便全面切换到的Firefox。那个时候Firefox推出的新版本Firefox Quantum也有一段时间了，是用Rust新写的内核，感觉确实比之前要快上不少（虽然还是不如Chrome快），加上界面改版的很漂亮，日常使用感觉还是很不错的。最近两天由于Firefox的中级证书过期，导致我这里好几个扩展都不能用了。其中就包括日常使用频率很高的SwitchyOmega。

<!--more-->

事实上Firefox自带了网络设置的选项，也没有必要再去用SwitchyOmega。当初尝试手动配置了一下Network Settings，不知为什么并没有生效，只好先继续使用着SwitchyOmega。本着奥卡姆剃刀原则，长时间以来一直想用自带的网络设置换掉SwitchyOmega，然而一直也没有付诸行动。现在Firefox扩展无法使用也终于促使我再次去拾起这个想法。

首先在配置好的SwitchyOmega上选择`Export PAC`，然后打开Firefox`Preferences`，下拉打开`Network Settings`，在弹出的对话框中选择`Automatic proxy configuration URL`，在对应的文本框中写入刚刚导出的PAC文件路径即可。

此时应该就能顺利看到Google了:)

UPDATE(2019.08.09)：最近偶有断流现象，不知为啥，还是先切换回了 Proxy SwitchyOmega ...
UPDATE(2019.08.10)：好象是 Firefox 的问题...