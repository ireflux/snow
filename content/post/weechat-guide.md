---
title: "WeeChat 不完全使用指南"
date: 2021-02-22
lastmod: 2021-02-23
draft: false
categories: ["Clavicula Salomonis"]
tags: ["linux", "WeeChat"]
author: "sherry"
---
前几天看了霍矩大佬做的一期关于 Clubhouse 视频，其中提到了 [IRC](https://en.wikipedia.org/wiki/Internet_Relay_Chat) 这种古老的通信协议。其中有一段叙述很有意思，大意是说如今 IRC 服务器这种带有很强匿名性质的聊天室，由于界面或操作还停留在上世纪，是天然的过滤屏障，人少反而提供了优质的社区交流氛围。但同时也不容易打击盗版，用户通过各种代理、中继连上来，很难去追溯，再加上现如今用户较少，规模不复往昔，因此可能也失去了打击的价值。种种技术和非技术的因素，造就了这样的一个强匿名、低审查、较为自由的一个“避难所”。用视频中的话说：“互联网上，没人知道你是一条狗”。

而这也正是吸引人的地方 :)

从 [arch wiki](https://wiki.archlinux.org/index.php/List_of_applications/Internet#IRC_clients) 中可以找到一些主流的支持 IRC 协议的软件。根据下面的图表对比，[WeeChat](https://WeeChat.org/) 相比于其他软件来说更强大，扩展性更好，最有优势。

<!--more-->

下图表搬运自 arch wiki：

| Name | Package | Written in | Extensible | [SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) |
| :---: | :---: | :---: | :---: | :---: |
| BitchX | bitchx-git | C | ? | ? |
| ERC | emacs | ELisp | in ELisp | via script |
| ii | iiAUR | C | stdin/stdout | No |
| Irssi | irssi | C | in Perl | Yes |
| pork | pork | C | in Perl | No |
| ScrollZ | scrollz | C | ? | No |
| sic | sicAUR | C | stdin/stdout | No |
| tiny | tiny-irc-client-git | Rust | No | Yes |
| WeeChat | WeeChat | C | multiple languages |Yes

### 启动 WeeChat

只需在终端中输入以下命令，回车即可：

```bash
$ WeeChat
```

如果你发现启动后出现了一些诸如 `Errors loading plugins` 之类的错误，是因为 WeeChat 启动时会默认加载这些插件，而你并未安装这些插件所需运行时所致。

如果想要消除这些错误信息，可以使用以下两种方法：

1. 安装 lua, ruby, aspell，tcl 等软件
2. 在 WeeChat 终端执行 `/set WeeChat.plugin.autoload "*,!ruby,!lua,!aspell,!tcl"` 就会在启动时阻止这些插件运行

WeeChat 本身是有一些界面交互命令，如 `version`，`uptime`，`print` 之类，如果没有插件，WeeChat 本身没什么用，什么都做不了。WeeChat 对 IRC 的支持也是通过插件来实现的，只不过 IRC 插件是默认内置的，如果想知道都加载了哪些插件，可以通过 `/plugin` 来查看。还可以在 [插件库](https://WeeChat.org/scripts/) 安装脚本插件。

### 在线手册

所有的命令都可以通过在 WeeChat 中输入 `/help` 来查询，如果想看某个命令的功能描述和用法，比如 `server`，可以像这样：

```
/help server
```

还可以查看某个配置项的描述和用法，比如 `irc.server.freenode.autojoin`：

```
/help irc.server.freenode.autojoin
```

### 设置选项

在 WeeChat 中查看所有的选项，使用 `/set` 命令，现在输入 `/set` 实际上是插件提供的指令 `/fset` 的 alias，还可以通过模糊匹配：

```
/fset WeeChat.*
```

可以通过如下命令来修改选项的值，例如：

```
/set irc.server.freenode.username "sherry"
```

修改后无需重启，WeeChat 会自动更新，或者使用 `/save` 强制保存修改后的值。不建议手动编辑配置文件来修改，因为 WeeChat 可能会随时写入信息。

### 添加服务器

以当前最大的节点 `chat.freenode.net` 为例，端口号为 6665-6667 和 8000-8002 用于纯文本连接，端口 6697, 7000 和 7070 为 TLS 加密连接。

不加密的那种我在国内尝试了下连不上，使用了加密连接才连上，写法如下，其中 `freenode` 是昵称：

```
/server add freenode chat.freenode.net/6697 -ssl
```

进去之后会发现自己的用户名旁边有个(+iZ)，这个代表用户模式，`i` 代表隐形，`Z` 是通过加密连接到服务器的用户，会自动获得这个网络状态，更多用户模式可以通过 `/help mode` 查看，或者浏览官方 Wiki [User Mode](https://freenode.net/kb/answer/usermodes)

### 自定义 IRC 服务器选项

如果您未为服务器选项指定特定值，则 WeeChat 会为所有服务器使用默认值。这些默认选项是 `irc.server_default.*`，因此对于每个服务器都可以单独设置参数。

默认情况下，昵称会和 un*x 的登陆用户名称相同，还可以修改昵称：

```
/set irc.server.freenode.nicks "sherry"
```

设置用户名和真实姓名：

```
/set irc.server.freenode.username "My user name"
/set irc.server.freenode.realname "My real name"
```

启动时自动连接服务器：

```
/set irc.server.freenode.autoconnect on
```

使用 SSL 加密连接：

```
/set irc.server.freenode.addresses "chat.freenode.net/7000"
/set irc.server.freenode.ssl on
```

如果服务器上有 [SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer)，可以将其用于身份验证：

```
/set irc.server.freenode.sasl_username "mynick"
/set irc.server.freenode.sasl_password "xxxxxxx"
```

连接到服务器时自动加入频道：

```
/set irc.server.freenode.autojoin "#channel1,#channel2"
```

重置所设置的选项：

```
/unset irc.server.freenode.nicks
```

设置其他选项（xxx为输入一个不存在的选项名）：

```
/set irc.server.freenode.xxx value
```

### 连接到 IRC 服务器

```
/connect freenode
```

### 加入/离开 IRC 频道

查询频道：

```
/list #channel
```

加入频道：

```
/join #channel
```

离开频道（界面不会关闭）：

```
/part [离开时的信息]]
```

关闭服务器/频道/私聊（同时界面也会关闭，`/close` 是 `/buffer close` 的 alias）：

```
/close
```

断开与服务器的连接：

```
/disconnect
```

## 后记

除了以上的一些基本用法，还有 [SASL 认证](https://weechat.org/files/doc/stable/weechat_user.en.html#irc_ssl_certificates) 以及 [使用 SASL 连接](https://freenode.net/kb/answer/sasl)、通过 [Tor 连接 Freenode](https://weechat.org/files/doc/stable/weechat_user.en.html#irc_tor_freenode) 等用法。此外，一些 WeeChat 软件的方面的问题，可以看这个 [FAQ](https://weechat.org/files/doc/stable/weechat_faq.en.html)

一些常用的 IRC 站点：

1. https://freenode.net
2. https://www.dal.net/servers
3. https://www.undernet.org

## 参考资料

1. [WeeChat Docs](https://weechat.org/doc/)
2. [Freenode Knowledge Base](https://freenode.net/kb/all)
3. [Arch Wiki IRC_clients](https://wiki.archlinux.org/index.php/List_of_applications/Internet#IRC_clients)
4. [Wikipedia SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer)