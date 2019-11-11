---
title: "i3gaps上触摸板的使用"
date: 2019-11-10
lastmod: 2019-11-10
draft: false
categories: ["随笔"]
tags: ["Linux"]
author: "sherry"
---
最近切换到了 i3gaps，发现触摸板的 tap to click 的功能不起作用，只有按键和二指滑动可用。看了下 Arch Wiki 后，发现只需要加一个配置文件就可以了。

在路径 `/etc/X11/xorg.conf.d/` 下创建例如 30-touchpad.conf 这样的配置文件，并其中写入如下配置项：

```bash
Section "InputClass"
        Identifier "touchpad"
        MatchIsTouchpad "on"
        Driver "libinput"
        Option "Tapping" "on"
        Option "TappingButtonMap" "lrm"
        Option "NaturalScrolling" "false"
        Option "ScrollMethod" "twofinger"
EndSection
```

注：在有些资料中 Driver 项可能是 synaptics，这是旧的驱动，已经不再更新了。

此外还有一些其他的选项，更多可参考 [libinput - libinput-based X.Org input driver](https://jlk.fjfi.cvut.cz/arch/manpages/man/libinput.4)

## 参考资料

> [archlinux libinput](https://wiki.archlinux.org/index.php/Libinput)  
[libinput - libinput-based X.Org input driver](https://jlk.fjfi.cvut.cz/arch/manpages/man/libinput.4)