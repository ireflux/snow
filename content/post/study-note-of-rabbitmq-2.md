---
title: "RabbitMQ 学习笔记 | 二" 
date: 2019-11-29
lastmod: 2019-11-29
draft: false
categories: ["Clavicula Salomonis"]
tags: ["RabbitMQ"]
author: "sherry"
---
开始填坑第二篇笔记，没想到这一隔就是将近四个月/笑。第一篇在这里：[RabbitMQ 学习笔记(1)](https://wanmei.ml/snow/post/study-note-of-rabbitmq-1/)

对于 Spring AMQP，消息默认是持久化的，前提是消息到达的终端队列也必须是持久的。尽管是持久化的，但是在消息接收的短时间内并未保存在磁盘上，而是在内存里。

RabbitMQ 有两种消息调度方式，一种是其默认的循环调度，即每个 consumer 都会收到相同数量的消息。另一种是公平调度，为 Spring AMQP 的默认配置。Spring AMQP 中 `AbstractMessageListenerContainer` 定义的 `DEFAULT_PREFETCH_COUNT` 值为 250,如果将其设置为 1,则将会变为循环调度。

<!--more-->

RabbitMQ 不指定 Exchange 也能发送到 Queue，是因为有默认的 Exchange，可使用 `sudo rabbitmqctl list_exchanges`命令查看。当使用默认的 Exchange 的时候，每个 Queue 都会以自己的名字作为 routing key 自动绑定到默认的 Exchange。一般情况下，我们会根据情景选择 Exchange 自行绑定来进行使用，以下列举出几个常见的 Exchange。

### Fanout exchange

这种 Exchange 可以一次性将消息发送到多个 consumer，也称为广播。但是无法灵活的控制消息发送，例如只发送到某个 consumer。此时就出现了另一个 Exchange，叫做 Direct exchange

### Direct exchange

可以发现，它可以在绑定 Queue 时多了个 `.with(routing key)` 选项，用以指定某个 routing key，发送消息时指定这个 routing key，消息便会被 Exchange 转发绑定的 Queue 中。但它仍然有局限性，就是不能以多个条件进行路由，不够灵活，因此 Topic exchange 出现了。

### Topic exchange

它的 routing key 是以多个单词组成的，单词之间以 `.` 隔开。因此我们就可以使用“通配符”来匹配，有以下两种“通配符”：

- \* 可以代替一个单词
- \# 可以代替零个或多个单词

可以看出，Topic exchange 是灵活度最高的，它甚至可以“退化”为 Direct exchange 和 Fanout exchange

由于它们使用起来都大同小异，因此不再展示代码，测试的时候可以通过 `sudo rabbitmqctl list_bindings` 来确认是否绑定了自定义的 exchange。根据官方的文档，还有一种exchange 叫做 headers，tutorial 中没有介绍，以后有用到的时候再补吧。

## 参考资料

[RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html)
