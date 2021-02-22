---
title: "使用 Github Actions 每天自动发送比特币的估值邮件"
date: 2019-12-24
lastmod: 2019-12-24
draft: false
categories: ["Clavicula Salomonis"]
tags: ["Github Actions"]
author: "sherry"
---

前段时间，在阮一峰老师那里看到了[GitHub Actions 入门教程](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)，于是自己也跑去申请了体验资格，然而玩了一下感觉太复杂就放弃了。时隔数日，Github Actions 正式发布了，阮老师又发布了一篇关于此的文章：[GitHub Actions 教程：定时发送天气邮件](http://www.ruanyifeng.com/blog/2019/12/github_actions.html)，这又让我想起了这个功能，于是决定重新捡起来。

Github Actions 对于个人用户来说确实是一个很棒的东西，因为它确实可以部分取代服务器，每个虚拟机的配置也足够：

- 2-core CPU
- 7 GB of RAM memory
- 14 GB of SSD disk space

<!--more-->

但为什么说“部分”呢？因为它还有一些限制，以下引用来自 github：

- You can execute up to 1000 API requests in an hour across all actions within a repository.
- Each job in a workflow can run for up to 6 hours of execution time.

如果需求不高，只是拿它来跑一些定时爬虫任务什么的还是很香的。

本次阮老师的文章里面用到了一个非常有趣的项目：[wttr.in](https://github.com/chubin/wttr.in)，于是我又去找了一些类似的好玩的项目：

1. [cheat.sh](https://github.com/chubin/cheat.sh)
2. [rate.sx](https://github.com/chubin/rate.sx)

在这个项目里，我使用 [rate.sx](https://github.com/chubin/rate.sx) 来每天向我发送各种币的估值信息以及比特币的估值曲线：

参考 rate.sx 的 README，直接获取各币种估值：

```bash
curl rate.sx
```

还可以获取单个币种的估值曲线如btc：

```bash
curl rate.sx/btc
```

接下来配置 actions，在项目中创建文件夹 `.github/workflows`，yml 配置文件必须放在 workflows 文件夹下：

```yml
name: 'Github Actions Bitcoin Bot'

on:
    push:
    schedule:
        - cron: '0 20 * * *'
```

1. name： workflow 的名称
2. on： 任务触发条件

以上触发方式为push以及定时任务，定时任务将在 UTC 时间 20 点执行，也就是北京时间 4 点，详情见[文档](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/events-that-trigger-workflows#scheduled-events-schedule)。

```yml
runs-on: ubuntu-latest
        steps:
            - name: Checkout codes
              uses: actions/checkout@v1
            - name: Get Result
              run: bash ./bitcoin.sh
```

1. runs-on: 运行环境，官方目前提供了[四种虚拟环境](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/virtual-environments-for-github-hosted-runners)：Windows Server 2019，Ubuntu 18.04，Ubuntu 16.04，macOS Catalina 10.15	
2. uses: 所使用的其他 actions，更多可见 [GitHub Actions](https://github.com/actions)

接下来是发送邮件:

```yml
- name: Send mail
uses: dawidd6/action-send-mail@master
    with:
    server_address: smtp.163.com
    server_port: 465
    username: ${{secrets.MAIL_USERNAME}}
    password: ${{secrets.MAIL_PASSWORD}}
    subject: Bitcoin Rate Report
    body: file://result.html
    to: mapleblue2007@gmail.com
    from: GitHub Actions
    content_type: text/html
```

以上的 username 和 password 可在仓库的 `Setting -> Secrets` 添加，还有更多用法可见[官方文档](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)。

发送邮件的用法可见 [dawidd6/action-send-mail](https://github.com/dawidd6/action-send-mail)

注： 开启 163 邮箱的 POP3/SMTP 服务，原来的密码处应填写为授权码。

## 参考资料

1. https://help.github.com/en/actions  
2. https://github.com/ruanyf/weather-action  
3. https://github.com/dawidd6/action-send-mail