---
title: "旧 Android 手机的再度营业" 
date: 2023-02-02
lastmod: 2023-02-02
draft: false
categories: ["随笔"]
author: "sherry"
---

## 背景

从前年开始，我就特别希望找一个体积小、方便携带、噪音小的单板计算机，类似于树莓派那种，打算平时拿来跑跑脚本什么的。但发现树莓派的价格比起过去简直翻了好几倍，于是果断放弃。当然按照习惯我也在网络上搜寻了[一些其他派](https://linux.cn/article-10823-1.html)，但也不知道好不好，就这样拖了好长时间。

前一段时间过年回家，发现家里有个旧手机红米4a，Android 6.0 还可以，但是是个移动合约机，玩了一下发现除了电池不太行以外其他各方面都很好。刚好想起来以前在酷安上看到有人用 Termux 装过 Ubantu，结合我之前的需求，可以说刚好满足，而且旧手机废物利用，拿来跑脚本再合适不过了

<!--more-->

## 安装(踩坑)过程

于是我重新线刷了一遍官方系统后，准备安装 Termux，发现 [Android 5 和 6 在 2020 年就不再支持了](https://www.reddit.com/r/termux/comments/dnzdbs/end_of_android56_support_on_20200101/)，一想到只能用旧阪本，虽然很不情愿，但也没有办法。不过后来我又在 WIKI 里发现了一篇新的公告：[Termux on android 5 or 6](https://github.com/termux/termux-app/wiki/Termux-on-android-5-or-6)，开发者又打算在 termux-app 0.119 版本中重新加回对 android 5 和 6 的支持，这么重要的信息居然不写在最显眼的 README 里面...虽然目前还没有正式 release，但是可以去 Actions 中找打好的安装包来安装，至此总算是可以舒舒服服的安装了。

作为 Arch 的忠实教徒，发行版当然是选择 Arch Linux 啦！在 GitHub 搜寻一番，找到了一个专门在 Termux 中一键安装的脚本 [TermuxArch](https://github.com/TermuxArch/TermuxArch)，总之到这里还算顺利。但接下来的安装遇到了一些错误，是关于 PRoot 的，提示没有 `--sysvipc` 的选项，可能是我的 PRoot 的版本的缘故，但用 Termux 自带的源尝试更新了一下，发现已经是最新的版本了。Hmmm...只好再次去 Termux 官网找答案，答案没找到，却在 Termux Wiki 找到了一篇关于 PRoot 的文章： [PRoot](https://wiki.termux.com/wiki/PRoot)，里面详细解释了 PRoot 和 Chroot 的区别，简单来说 PRoot 是通过 ptrace() 来劫持系统调用参数和返回值来伪造可见的文件系统布局或用户/组的 ID，会引起额外的开销，里面还提到了一个 APP： [Linux Deploy](https://github.com/meefik/linuxdeploy)。考虑到我的手机性能，开销这种东西还是能少一点就少一点，于是果断选择换方案。

接下来就是获取 root 权限的过程了：从线刷包里提取 boot.img -> magisk 修补 boot.img -> `fastboot flash boot boot.img`

装完 Linux Deploy 后，本以为接下来会顺利一点，但没想到仍然遇到了一些坑。安装选的 Arch，但每次装到 gcc-debug 这个包这里就会 fail，然后就没有然后了，无论是官方的源还是国内镜像源，都是同样的错误。考虑到这个软件停更了，最新的版本还停留在 2020 年 2 月份，两年过去了，有一些 bug 也说不定。之后各处搜寻，最终还是找到了解决方案。

Linux Deploy 直接安装报错 log:

```
[11:40:17] >>> deploy
[11:40:17] :: Installing bootstrap/rootfs ... 
[11:40:17] Checking installation path ... done
[11:40:17] Making new disk image (2047 MB) ... done
[11:40:17] Making file system (ext4) ... done
[11:40:17] Checking file system ... done
[11:40:17] Mounting the container: 
[11:40:17] / ... done
[11:40:17] /proc ... done
[11:40:17] /sys ... done
[11:40:17] /dev ... done
[11:40:17] /dev/shm ... skip
[11:40:17] /dev/pts ... done
[11:40:17] /dev/tty ... done
[11:40:17] /proc/sys/fs/binfmt_misc ... done
[11:40:17] :: Installing bootstrap/archlinux ... 
[11:40:17] Preparing for deployment ... done
[11:40:20] Retrieving packages list ... done
[11:40:20] Retrieving packages: 
[11:40:22] acl-2.3.1-2 ... done
[11:40:25] acl-debug-2.3.1-2 ... done
[11:40:28] amd-ucode-20221214.f3c283e-1 ... done
[11:40:31] archlinux-keyring-20221220-1 ... done
[11:40:33] archlinuxarm-keyring-20140119-2 ... done
[11:40:35] argon2-20190702-4 ... done
[11:40:38] attr-2.5.1-2 ... done
[11:40:40] attr-debug-2.5.1-2 ... done
[11:40:42] audit-3.0.8-1 ... done
[11:40:45] audit-debug-3.0.8-1 ... done
[11:40:48] autoconf-2.71-1 ... done
[11:40:51] automake-1.16.5-1 ... done
[11:40:52] b43-fwcutter-019-3 ... done
[11:40:54] base-3-1 ... done
[11:40:58] bash-5.1.016-1 ... done
[11:41:05] binutils-2.38-5.1 ... done
[11:41:27] binutils-debug-2.38-5.1 ... done
[11:41:30] bison-3.8.2-4 ... done
[11:41:33] brotli-1.0.9-9 ... done
[11:41:37] brotli-debug-1.0.9-9 ... done
[11:41:42] brotli-testdata-1.0.9-9 ... done
[11:41:45] btrfs-progs-6.1-2 ... done
[11:41:49] btrfs-progs-debug-6.1-2 ... done
[11:41:51] bzip2-1.0.8-5 ... done
[11:41:53] bzip2-debug-1.0.8-5 ... done
[11:41:55] ca-certificates-20220905-1 ... done
[11:41:57] ca-certificates-cacert-20140824-4 ... done
[11:42:00] ca-certificates-mozilla-3.86-1 ... done
[11:42:02] ca-certificates-utils-20220905-1 ... done
[11:42:07] coreutils-9.1-3 ... done
[11:42:09] cracklib-2.9.8-1 ... done
[11:42:12] cronie-1.6.1-1 ... done
[11:42:14] cronie-debug-1.6.1-1 ... done
[11:42:16] cryptsetup-2.6.0-1 ... done
[11:42:19] cryptsetup-debug-2.6.0-1 ... done
[11:42:22] curl-7.87.0-1 ... done
[11:42:26] curl-debug-7.87.0-1 ... done
[11:42:28] dash-0.5.11.5-1 ... done
[11:52:10] dash-debug-0.5.11.5-1 ... done
[11:52:10] db-6.2.32-1 ... done
[11:52:10] db5.3-5.3.28-2 ... done
[11:52:10] dbus-1.14.4-1 ... done
[11:52:10] dbus-debug-1.14.4-1 ... done
[11:52:10] dbus-docs-1.14.4-1 ... done
[11:52:10] debugedit-5.0-4 ... done
[11:52:10] debuginfod-0.188-1 ... done
[11:52:10] device-mapper-2.03.18-1 ... done
[11:52:10] dhcpcd-9.4.1-1 ... done
[11:52:10] dialog-1:1.3_20220728-1 ... done
[11:52:10] diffutils-3.8-1 ... done
[11:52:10] ding-libs-0.6.2-1 ... done
[11:52:10] dmraid-1.0.0.rc16.3-14 ... done
[11:52:10] dnssec-anchors-20190629-3 ... done
[11:52:10] dosfstools-4.2-2 ... done
[11:52:10] e2fsprogs-1.46.5-4 ... done
[11:52:10] e2fsprogs-debug-1.46.5-4 ... done
[11:52:10] ed-1.18-1 ... done
[11:52:10] efivar-38-2 ... done
[11:52:10] elfutils-0.188-1 ... done
[11:52:10] elfutils-debug-0.188-1 ... done
[11:52:10] expat-2.5.0-1 ... done
[11:52:10] expat-debug-2.5.0-1 ... done
[11:52:10] fakeroot-1.30.1-1 ... done
[11:52:10] file-5.44-1 ... done
[11:52:10] file-debug-5.44-1 ... done
[11:52:10] filesystem-2022.10.18-1 ... done
[11:52:10] findutils-4.9.0-1 ... done
[11:52:10] flex-2.6.4-3 ... done
[11:52:10] fuse2fs-1.46.5-4 ... done
[11:52:10] gawk-5.2.1-1 ... done
[11:52:10] gawk-debug-5.2.1-1 ... done
[11:52:10] gc-8.2.2-1 ... done
[11:52:10] gcc-12.1.0-2.1 ... done
[11:52:10] gcc-debug-12.1.0-2.1 ... fail
[11:52:10] <<< deploy
```

首先要自己先把 Archlinuxarm 系统镜像下载下来，这个随便找个国内的镜像就好，比如 [USTC](https://mirrors.ustc.edu.cn/archlinuxarm/os/)，然后打开 Linux Deploy，发行版选择 `rootfs.tar`，源地址写 `${EXTERNAL_STORAGE}/[下载的镜像名称]`，安装类型选择 `目录`，安装路径写 `${ENV_DIR}/rootfs/linux`，其他选项就随意了，记得勾上 `启用SSH` 就好。p.s. `${EXTERNAL_STORAGE}` 事实上就是 `/storage/emulated/0`，`${ENV_DIR}` 就是 `/data/data/[Linux Deploy包名]/`

接下来顺利安装完成，ssh 进入 Arch，默认的 DNS 有问题，删掉 resolv.conf 重建。然后开始安装[青龙面板](https://github.com/whyour/qinglong)，直接按照 [青龙 README](https://github.com/whyour/qinglong#%E5%BC%80%E5%8F%91) 来启动就好了。

完结。

顺便说一句，青龙真吃内存...

## 参考资料

1. 12 个可替代树莓派的单板机: https://linux.cn/article-10823-1.html
2. End of android-5/6 support on 2020-01-01: https://www.reddit.com/r/termux/comments/dnzdbs/end_of_android56_support_on_20200101/
3. Termux on android 5 or 6: https://github.com/termux/termux-app/wiki/Termux-on-android-5-or-6
4. TermuxArch: https://github.com/TermuxArch/TermuxArch
5. PRoot: https://wiki.termux.com/wiki/PRoot
6. Linux Deploy: https://github.com/meefik/linuxdeploy
7. 青龙面板: https://github.com/whyour/qinglong