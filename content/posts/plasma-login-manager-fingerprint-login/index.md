---
title: "Plasma Login Manager 指纹登录"
date: 2026-03-17T09:20:22+08:00
categories: ["Linux"]
tags: ["Linux", "KDE", "Plasma"]
---

在现代 Linux 系统中，基本都是使用 [fprint](https://fprint.freedesktop.org/) 来便捷的使用指纹识别的功能，其内置的 PAM 模块可以用于用户指纹登录系统。
<!--more-->

## 使用前提

通过 lsusb 命令找到指纹识别设备，对比 [fprint 的兼容设备清单](https://fprint.freedesktop.org/supported-devices.html)来确定设备是否已支持，不支持的设备就没法使用了。

## 安装

Arch Linux 下使用 sudo pacman -S fprintd libfprint 命令进行安装。

启动 fprintd 服务并设置为开机自启动：

```bash
sudo systemctl start fprintd.service
sudo systemctl enable fprintd.service
```

可能会提示启动依赖的问题，使用命令 sudo systemctl edit --full --force fprintd.service 修改服务配置文件：

```ini
[Install]
WantedBy=multi-user.target
```

使用命令 sudo systemctl daemon-reload 重新加载服务配置文件就可以解决问题了。

## 录入指纹

运行 fprintd-enroll 命令就可以为当前用户录入指纹，默认是录入右手食指指纹，也可以指定录入不同的手指指纹，参考[命令行手册](https://man.archlinux.org/man/fprintd.1)。

```bash
fprintd-enroll -f right-index-finger
fprintd-enroll -f right-middle-finger
```

录入完成后，可以使用 fprintd-verify -f _finger_ 来验证指纹，使用 fprintd-delete -f _finger_ 来删除指纹。

## 配置指纹登录

要在 Plasma Login Manager 中使用指纹登录，需要修改 /etc/pam.d/plasmalogin 配置文件，如果没有就从 /usr/lib/pam.d/plasmalogin 拷贝一个过来。

添加下面两行配置到开头位置，这样就可以使用密码或者指纹识别进行登录。

```text
...
# SPDX-License-Identifier: CC0-1.0
# SPDX-FileCopyrightText: none

auth        sufficient  pam_unix.so try_first_pass likeauth nullok
auth        sufficient  pam_fprintd.so
...
```

在系统启动登录界面或锁屏界面时，直接敲击回车键，就可以使用指纹认证登录了。

{{< alert >}}
🔵 默认情况下指纹登录时不会解锁 KWallet ，使用 KWallet 时会弹出密码输入框进行认证，自动解锁可以参考 [KWallet Wiki 指导](https://wiki.archlinuxcn.org/wiki/KDE_Wallet#%E7%99%BB%E5%BD%95%E6%97%B6%E8%87%AA%E5%8A%A8%E8%A7%A3%E9%94%81_Kwallet)。
{{< /alert >}}
