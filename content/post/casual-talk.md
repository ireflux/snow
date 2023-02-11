---
title: "杂谈"
date: 2019-04-17
lastmod: 2019-05-02
draft: false
categories: ["随笔"]
author: "sherry"
---
近日全面将 Shadowsockes 向 v2ray 切换，原本 Arch 上用着 `sslocal -c /path/filename.config` 还挺舒服的，配置文件看起来也比 v2ray 要简单得多，无奈 Shadowsocks 最近表现的一直不大稳定，总是被封 IP。

v2ray 的服务端和客户端是同一套程序，配置成服务端还是客户端取决于配置文件中 `inbounds` 和 `outbounds` 的配置，一般来说 v2ray 官网的 `新手上路` 中的配置已经足够日常使用。但是为了稳妥起见，还是要进行进一步配置。例如采用 websocket+TLS+Web 这种方案。

<!--more-->

linux 上客户端的使用最简单的方式就是找一些配置文件改改来用。例如：可以在线生成配置文件的[配置文件生成器](https://www.veekxt.com/utils/v2ray_gen);再比如 V2Ray 配置文件模板收集仓库 [Github](https://github.com/KiriKira/vTemplate);还可以直接导入到 Windows GUI 程序中，然后再导出配置文件来用。

- - -

前几天 python 连接 docker 中的 redis 的时候发现总是出现 `Connection refused` ，刚开始还以为 docker 网络的配置原因，后来才发现原来是 redis 配置文件的缘故。将 `bind 127.0.0.1` 着一条注释掉就好了 Hmmm

- - -

今天照例滚系统，在安装 Wps office 的时候 yaourt 提示 `No space left on device`，看了一下，原来是 `/tmp` 空间不够。搜寻了一圈之后发现两个解决方案：

1. mount -o remount,size=5G /tmp/
2. umount /tmp && mount -t tmpfs -o size=1048576,mode=1777 overflow /tmp

第一种要比第二种简洁很多，不过第二种的 `mode=1777` 没看懂是什么意思，随后去查了 man page，以下引用自 man page：

> Set the mode of all files to value & 0777 disregarding the original permissions. Add  search  permission to directories that have read permission. The value is given in octal.

大概就是类似于 `chmod` ，至于为什么会是 `1777` ，以下引用 [wikipedia](https://en.wikipedia.org/wiki/Sticky_bit):

> When a directory's sticky bit is set, the filesystem treats the files in such directories in a special way so only the file's owner, the directory's owner, or root user can rename or delete the file. Without the sticky bit set, any user with write and execute permissions for the directory can rename or delete contained files, regardless of the file's owner. Typically this is set on the /tmp directory to prevent ordinary users from deleting or moving other users' files. 

大致意思就是说一旦设置了 `sticky bit` ，只有目录/文件的所有者或者 root 账户才能对文件进行写操作，其他的用户即使获得了对这个目录写操作权限，也无法对其进行写操作。

之后又去 archlinux wiki 上看了看，发现 yaourt 开发者在2018年11月10号的一条 [issue](https://github.com/archlinuxfr/yaourt/issues/382#issuecomment-437461631) 中表示无意再继续维护 yaourt 了，之后 2018 年 11 月 30 号被 aur helper 页面标记为停更，2019 年 3 月 25 号从 aur help 中被移除。

看了看发现大家比较推荐 yay，自己 `makepkg -si` 的话确实会麻烦一点，想了一下还是换成了 yay。另一方面 archlinuxcn 源也能解决不少 aur 里的东西，于是再次添加上了 archlinuxcn 源。

不得不说 wps for linux 新版本 UI 变漂亮了好多。

嗯，真香。

## 参考资料

1. [Project V](https://www.v2ray.com/)  
2. [Sticky_bit](https://en.wikipedia.org/wiki/Sticky_bit)  
3. [aur helper](https://wiki.archlinux.org/index.php/AUR_helpers)