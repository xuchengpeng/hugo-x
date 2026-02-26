---
title: "KDE 启用 Plasma Login Manager"
date: 2026-02-26T14:19:52+08:00
categories: ["Linux"]
tags: ["Linux", "Arch", "KDE", "Plasma"]
---

> Plasma Login provides a display manager for KDE Plasma, forked from [SDDM](https://github.com/sddm/sddm) and with a new frontend providing a greeter, wallpaper plugin integration and System Settings module (KCM).
<!--more-->

随着 KDE Plasma 6.6 的正式发布，可以使用 Plasma Login Manager 作为登录器。

首先，按照以下命令安装和启用：

```bash
sudo pacman -S plasma-login-manager
sudo systemctl disable sddm.service
sudo systemctl enable plasmalogin.service
```

最后，重启系统就可以使用新的登录器了。
