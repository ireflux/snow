---
title: "Linux上adb的使用记录" 
date: 2019-05-02
lastmod: 2019-05-02
draft: false
categories: ["Clavicula Salomonis"]
tags: ["Linux", "adb"]
author: "sherry"
---
## 前言

我对手机的需求不高，不打游戏，只需要续航，有Google服务以及root就足够了。之前一直使用的ADUI的Global版本，再加上Magisk的root管理，配上冰箱也足以应付诸如微信这一类的国产毒瘤了。但之前有一次升级到Pie的大版本更新，把我的root覆盖掉了。因此，不得不重新再刷入Magisk。不过MIUI11官方说会加入应用冻结的功能，这样一来我连root的需求似乎也没有了 /笑

言归正传，在Linux上使用adb有些机型还是需要做一些配置，不然 `adb devices` 会显示：

> List of devices attached  
[device name]    unauthorized

<!--more-->

要进行配置之前，首先需要找到自己手机的USB vendor ID和product ID，启用开发者模式，然后打开usb调试模式插上手机后在terminal中执行：

```bash
$ lsusb
```

它将会显示出好几条类似 `Bus xxx Device xxx: ID xxxx:xxxx ` 的东西，根据后面显示的公司信息很容易分辨出自己的手机是哪一个，ID 之后的 `xxxx：xxxx`，前者是vendor id，后者是product id

下面有两种方式来写配置文件：

### 添加udev规则

1.首先要确保系统中有安装 `android-udev` 这个包。可以通过包管理器来安装或者访问[source.android.com](https://source.android.com/setup/build/initializing#configuring-usb-access)来手动安装。在这里新建一个文件：`/etc/udev/rules.d/51-android.rules`，然后写入以下配置，只需修改其中的vendor id和product id为自己的即可:

> SUBSYSTEM=="usb", ATTR{idVendor}=="[VENDOR ID]", MODE="0660", GROUP="adbusers"  
SUBSYSTEM=="usb",ATTR{idVendor}=="[VENDOR ID]",ATTR{idProduct}=="[PRODUCT ID]",SYMLINK+="android_adb"  
SUBSYSTEM=="usb",ATTR{idVendor}=="[VENDOR ID]",ATTR{idProduct}=="[PRODUCT ID]",SYMLINK+="android_fastboot"

2.然后重新载入新的配置文件：

```bash
$ sudo udevadm control --reload-rules
```

3.然后将当前的用户添加到 `adbusers` 这个组里：

```bash
$ sudo gpasswd -a user group
```

### 配置adb

在这个位置创建文件 `~/.android/adb_usb.ini` ，然后将vendor id 写入到里面，保存退出即可。

这两种方式任选其一即可。然后进入解压后的platform-tools目录，将下好的twrp也放到里面，在目录中启动terminal，然后执行：

```bash
$ ./adb devices
```

这样的话，应该就能看到设备被列出来了，类似于这样：

> List of devices attached  
[device name]    device

这样就算是完成了。

## 后记

我自己使用的是第二种方式，能够成功找到adb。`adb reboot bootloader` 后进入bootloader也能通过 `fastboot devices` 找到设备，然而通过 `fastboot flash recovery xxxx.img` 就会莫名其妙的卡住...通过 `fastboot boot xxxx.img` 想要直接进入 twrp 也会卡在 sending...这里。不知道是什么原因，先挖个坑，以后再慢慢研究好了。

## 参考资料

1. [Android Debug Bridge](https://wiki.archlinux.org/index.php/Android_Debug_Bridge)