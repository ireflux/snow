---
title: "杂谈"
date: 2019-4-17T21:10:00+8:00
lastmod: 2019-4-17T21:10:00+8:00
draft: false
categories: ["随笔"]
author: "sherry"
---
近日全面将Shadowsockes向v2ray切换，原本Arch上用着 `sslocal -c /path/filename.config` 还挺舒服的，配置文件看起来也比v2ray要简单得多，无奈Shadowsocks最近表现的一直不大稳定，总是被封IP。

v2ray的服务端和客户端是同一套程序，配置成服务端还是客户端取决于配置文件中 `inbounds` 和 `outbounds` 的配置，一般来说v2ray官网的 `新手上路` 中的配置已经足够日常使用。但是为了稳妥起见，还是要进行进一步配置。例如采用websocket+TLS+Web这种方案。

<!--more-->

linux上使用最简单的方式就是找一些配置文件改改来用。例如：可以在线生成配置文件的[配置文件生成器](https://www.veekxt.com/utils/v2ray_gen);再比如 V2Ray 配置文件模板收集仓库 [Github](https://github.com/KiriKira/vTemplate);还可以直接导入到Windows GUI程序中，然后再导出配置文件来用。

...

前几天python连接docker中的redis的时候发现总是出现 `Connection refused` ，刚开始还以为docker网络的配置原因，后来才发现原来是redis配置文件的缘故。将 `bind 127.0.0.1` 着一条注释掉就好了 Hmmm

...

今天照例滚系统，在安装Wps office的时候yaourt提示 `No space left on device`，看了一下，原来是/tmp空间不够。搜寻了一圈之后发现两个解决方案：

1. mount -o remount,size=5G /tmp/
2. umount /tmp && mount -t tmpfs -o size=1048576,mode=1777 overflow /tmp

第一种要比第二种简洁很多，不过第二种的 `mode=1777` 没看懂是什么意思，随后去查了man page，以下引用自man page：

> Set the mode of all files to value & 0777 disregarding the original permissions. Add  search  permission to directories that have read permission. The value is given in octal.

大概就是类似于 `chmod` ，至于为什么会是 `1777` ，以下引用 [wikipedia](https://en.wikipedia.org/wiki/Sticky_bit):

> When a directory's sticky bit is set, the filesystem treats the files in such directories in a special way so only the file's owner, the directory's owner, or root user can rename or delete the file. Without the sticky bit set, any user with write and execute permissions for the directory can rename or delete contained files, regardless of the file's owner. Typically this is set on the /tmp directory to prevent ordinary users from deleting or moving other users' files. 

大致意思就是说一旦设置了 `sticky bit` ，只有目录/文件的所有者或者root账户才能对文件进行写操作，其他的用户即使获得了对这个目录写操作权限，也无法对其进行写操作。

之后又去 archlinux wiki 上看了看，发现yaourt开发者在2018年11月10号的一条[issue](https://github.com/archlinuxfr/yaourt/issues/382#issuecomment-437461631)中表示无意再继续维护yaourt了，之后2018年11月30号被aur helper页面标记为停更，2019年3月25号从aur help中被移除。

于是我也删掉了yaourt，换成了yay，再次添加上了archlinuxcn源。不得不说 Wps for linux 新版本UI变漂亮了好多。

嗯，真香:)

## 参考资料

> [Project V](https://www.v2ray.com/)
> [Sticky_bit](https://en.wikipedia.org/wiki/Sticky_bit)
> [aur helper](https://wiki.archlinux.org/index.php/AUR_helpers)