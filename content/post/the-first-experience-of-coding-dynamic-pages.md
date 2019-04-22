---
title: "Coding动态Pages初体验"
date: 2018-2-5T21:53:00+8:00
lastmod: 2018-2-5T21:53:00+8:00
draft: false
categories: ["随笔"]
author: "sherry"
---
# 前言

前一段时间发现国内的Coding可以部署动态Pages，意味着不需要购买服务器就可以免费得到一个带评论的动态博客，还是很不错的，毕竟Github的静态Pages要评论的话只能借助第三方服务，而且Coding在国内，速度上有天然优势。

## 体验

于是美滋滋的注册了一个帐号，现在的注册人数应该不多，三位数纯英文的用户名都可以轻松注册的到（最低要求三位），想当年注册个Google帐号六位纯英文的已经很难了，而且各种奇葩丑到爆六位英文组合都被人抢注了……

<!--more-->

之后进入了主界面，不得不说，虽然界面和Github比较像，但这不是重点，重点是那个WEBIDE的DEMO试玩了一下，真的是AWESOME，在来就是那个动态Pages了，启动部署后发现网址居然是好长（>30位）的一个随机码.coding.io，果然如官网所说会得到一个”xxx.coding.io“的网址:(，之前还以为是用户名+coding.io，就像Github的静态页面一样。现在这就比较坑了，不搞个域名都没什么实用的价值……然而自定义域名需要银牌会员以上，想成为银牌会员需要完善身份信息，然后还是受限制的自定义域名，每次访问都会出现“该网站托管在 Coding Pages 上” ，然后再跳转到原站。想要不受限制，需要升级到金牌会员以上，也就是充值199/year……

免费的东西果然还是不能想太多，考虑到WordPress过于臃肿，于是我选择了Typecho，Coding有个官方的pages-demo项目，其中README中截图显示可以在创建仓库时直接从github开源项目中导入仓库，我认真的找了下，骗子！明明没有这个选项！（不知道是不是被官方在后期更新中砍掉了)，只好自己推了，从官网下载后解压，原本想直接上传上去，结果却提示有非法字符，后来发现文件中似乎有一些"@"字符，这样的话只能用Git了。

最后用Git推送到Coding时，出现了一个错误，如下所示：

>
error: failed to push some refs to '../xxx.git'  
hint: Updates were rejected because the remote contains work that you do  
hint: not have locally. This is usually caused by another repository pushing  
hint: to the same ref. You may want to first integrate the remote changes  
hint: See the 'Note about fast-forwards' in 'git push --help' for details.  

原因可能是因为创建Coding仓库的时候带了一个README，和本地仓库不同步，可能需要git pull一下，我当时没试，直接git push -f origin master强制推送上去了。

最后重新部署一下，访问网址，然后根据动态Pages中所提供的数据库连接信息填写数据库信息，安装成功！