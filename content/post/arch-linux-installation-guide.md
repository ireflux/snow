---
title: "ArchLinux 安装教程"
date: 2017-10-18
lastmod: 2023-07-15
draft: false
categories: ["随笔"]
tags: ["Linux"]
author: "sherry"
---
体验了 Win10 一个多月后，感觉不光系统臃肿，还有些迷之 BUG，首先是点关机后，系统实际上并未关闭，根据网上的方法禁用了 intel management engine interface 驱动后解决。另一个是看视频暂停后，再继续声音会突然变大，只能在系统音量那随便调一下就正常。虽然并不怎么影响使用，但作为完美主义者这是绝对无法忍受的。曾数次 Google，无论是禁用声音效果还是改声卡驱动都无济于事……

心灰意懒，于是在使用了 Win10 一个多月后，我又决定要换系统了:D，这大概是我使用 Windows 系统以来时间最短的一个版本了吧（笑。

<!--more-->

为了方便，还是自己重新写一个吧，基本上是从官网复制一遍，省的每次都要去翻官网的[文档](https://wiki.archlinux.org/title/Installation_guide)，要翻好多篇文章。以下制作启动盘等准备工作省略，直接从进入 live 环境开始。

## 安装前的准备工作

### 联网

网线直接 `dhcpd`

如果没有网线，直接手机开 USB 共享网络，插到电脑上，然后再 `dhcpd`

### 更新系统时钟

默认情况下连上网络就会自动更新的，可以用以下命令查看一下：
```bash
# timedatectl
```

### 分区

这里只写使用 64-bit x64 UEFI 引导启动的情况，毕竟 2023 年了，32-bit IA32 UEFI 和 MBR 越来越少了

还是要查看一下启动模式：
```bash
# cat /sys/firmware/efi/fw_platform_size
```

返回结果的解释，这里就直接复制官网了：

> If the command returns 64, then system is booted in UEFI mode and has a 64-bit x64 UEFI. If the command returns 32, then system is booted in UEFI mode and has a 32-bit IA32 UEFI; while this is supported, it will limit the boot loader choice to GRUB. If the file does not exist, the system may be booted in BIOS (or CSM) mode. If the system did not boot in the mode you desired (UEFI vs BIOS), refer to your motherboard's manual.

如果上面的命令返回 64 的话，就可以继续了，其他情况可以 return 了

先查看分区：
```bash
# fdisk -l
```

就会看到磁盘例如 `/dev/sda`, `/dev/sdb` ，或者已有的分区例如 `/dev/sda1`,`/dev/sda2`

假设 `/dev/sda` 是一个全新的、未创建分区表的磁盘，输入以下命令开始操作 `/dev/sda` ：
```bash
# fdisk /dev/sda
```

上述命令执行完后，输入 `m` 可以查看 help

首先创建 UEFI 引导分区，输入 `g` 创建一个 GPT 分区表，然后输入 `n` 新建分区，起始序号和 First sector 默认回车就行，Last sector 输入 `+512M` 回车，创建完默认是类型 `Linux filesystem`，输入 `p` 可以查看新创建的分区，输入 `t` 选择分区序号来修改分区类型，输入 `l` 可以查看[所有的类型](https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_type_GUIDs)，输入 `uefi` 将分区类型修改为 EFI。然后再次输入 `n` 创建 root 分区，一路回车默认即可，最后输入 `w` 保存

保存完，再次执行 `fdisk -l` 就会看到类似如下结果：
| Device | Start | End | Sectors | Size | Type |
|:---:|:---:|:---:|:---:|:---:|:---:|
| /dev/sda1 | 2048 | 1050623 | 1048576 | 512M | EFI System |
| /dev/sda2 | 省略 | 省略 | 省略 | 省略 | Linux filesystem |

### 格式化分区

格式化 UEFI 分区建议使用 FAT32 格式，详情见官网文档[EFI_system_partition#Format_the_partition](https://wiki.archlinux.org/title/EFI_system_partition#Format_the_partition)，具体我也摘抄如下：

> To prevent potential issues with other operating systems and since the UEFI specification says that UEFI "encompasses the use of FAT32 for a system partition, and FAT12 or FAT16 for removable media"[5], it is recommended to use FAT32.

所以格式化 EFI 分区使用如下命令:
```bash
# mkfs.fat -F 32 /dev/sda1
```

然后格式化 root 分区：
```bash
# mkfs.ext4 /dev/sda2
```

### 挂载文件系统

```bash
# mkdir /mnt/boot
# mount /dev/sda1 /mnt/boot
# mount /dev/sda2 /mnt
```

## 安装

### 选择镜像

编辑以下文件：
```bash
# vim /etc/pacman.d/mirrorlist
```

将 `Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch` 放到第一行，保存退出

### 安装必要的软件包

```bash
# pacstrap -K /mnt base linux linux-firmware
```

## 系统配置

### 生成 fstab 文件

执行下面的命令:
```bash
# genfstab -U /mnt >> /mnt/etc/fstab
```

查看生成的结果:
```bash
# cat /mnt/etc/fstab
```

### Chroot

执行以下命令切换到新系统:
```bash
# arch-chroot /mnt
```

### 设置时区

```bash
# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

生成 `/etc/adjtime` 文件：
```bash
# hwclock --systohc
```

### 本地化

编辑文件 `/etc/locale.gen`，找到 `en_US.UTF-8 UTF-8` 这一行并取消注释，然后保存

执行以下命令生成 locales：
```bash
# locale-gen
```

编辑文件 `/etc/locale.conf`，写入 `LANG=en_US.UTF-8`

### 网络配置

创建 `/etc/hostname` 文件:
```bash
# echo "arch" >> /etc/hostname
```

编辑 `/etc/hosts` 文件:
```bash
# echo -e "127.0.0.1 localhost\n::1 localhost\n127.0.1.1 arch.localdomain arch"
```

### 修改密码

```bash
# passwd
```

### Boot loader

不同的 CPU 需要安装不同的 Microcode：

AMD CPU 安装：
```bash
# pacman -S amd-ucode 
```

Intel CPU 安装：
```bash
# pacman -S intel-ucode
```

接下来安装 GRUB：
```bash
# pacman -S grub efibootmgr
```

然后执行：
```bash
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

配置 GRUB：
```bash
# grub-mkconfig -o /boot/grub/grub.cfg
```

### 安装软件

顺便再装一些必要的软件:
```bash
# pacman -S sudo vim dhcpd netctl wpa_supplicant
```

### 重启

```bash
# exit
# umount /mnt/boot
# umount /mnt
# reboot
```

## 参考资料

1. https://wiki.archlinux.org/title/Installation_guide  
2. https://wiki.archlinux.org/title/General_recommendations
