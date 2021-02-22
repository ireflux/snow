---
title: "RabbitMQ 学习笔记 | 一" 
date: 2019-08-05
lastmod: 2019-08-05
draft: false
categories: ["Clavicula Salomonis"]
tags: ["RabbitMQ"]
author: "sherry"
---
此文仅作个记录

开始之前，首先需要安装 RabbitMQ 的服务端，由于我此次的安装环境为 Debian，根据官网所说，推荐使用 [apt repository on Package Cloud or Bintray](https://www.rabbitmq.com/install-debian.html#apt)这种方式来安装，然而在这里还是遇到了一些问题，想要安装最新的 RabbitMQ，则需要高版本的 Erlang，具体详情可以看这个关于 RabbitMQ 和 Erlang/OPT 的兼容性[表格](https://www.rabbitmq.com/which-erlang.html#compatibility-matrix)

<!--more-->

然而 Debian 的 Erlang 版本很旧，因此还需要先安装 Erlang，关于[这一点](https://www.rabbitmq.com/which-erlang.html#debian)，官网上也提到了。因此也给出了[解决方案](https://www.rabbitmq.com/install-debian.html#erlang-repositories)，安装过程此处不再赘述。

安装完毕之后，其他一些需要注意的地方：比如说默认情况下端口号是 5672，HTTP的端口是 15672，更多默认端口可以参考[这里](https://www.rabbitmq.com/install-debian.html#ports)。RabbitMQ 自带了一个管理界面，如若要开启这个 WEB 界面，需要开启一个插件，只需要输入以下命令即可开启：

```bash
$ rabbitmq-plugins enable rabbitmq_management
```

然后访问 `ip:15672` 这个地址，这样就可以愉快的使用图形化界面来管理 RabbitMQ 了，更多详情还可以看[这里](https://www.rabbitmq.com/management.html)。

默认情况下还会有一个默认的账户用于登录，账户名和密码都是：guest，而且只能从本机上连接。一般正常使用过程中都是需要装在服务器上，然后远程连接的，新创建一个账户就没有这个限制了。可以使用上面那个图形化界面来添加账户，也可以使用 rabbitmqctl 来创建。命令如下：

```bash
$ rabbitmqctl add_user username password  
$ rabbitmqctl set_user_tags username administrator  
$ rabbitmqctl set_permissions -p / username ".*" ".*" ".*"
```

以上命令大概看英文就知道命令是做什么用的，几乎不用解释，更多关于权限控制的可以看[这里](https://www.rabbitmq.com/access-control.html)

还有一个关于配置的文档，里面包含很多配置信息，以及很多基础配置的默认值都可以在这里找到，戳[这里](https://www.rabbitmq.com/configure.html)查看。

不得不说，RabbitMQ 的文档还是很全面的，基本上遇到的问题都可以在官方文档上找到想要的信息。

## 参考资料

1. [RabbitMQ Documentation](https://www.rabbitmq.com/documentation.html)
