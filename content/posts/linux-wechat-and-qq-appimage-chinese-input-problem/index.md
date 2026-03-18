---
title: "Linux WeChat 和 QQ AppImage 中文输入问题"
date: 2026-03-18T08:53:59+08:00
categories: ["Linux"]
tags: ["Linux", "RIME"]
---

AppImage 程序因其打包机制通常与宿主机输入法框架（如 Fcitx5 或 IBus）隔离。要在 AppImage 中使用中文输入法，关键在于运行程序手动显式加载 fcitx 环境。
<!--more-->

可以封装一个 shell 脚本设置相关环境变量再运行 AppImage ，也可以修改程序对应 desktop 文件来设置环境变量。

```desktop
[Desktop Entry]
Name=WeChat
Exec=env QT_IM_MODULE=fcitx XMODIFIERS=@im=fcitx GTK_IM_MODULE=fcitx ELECTRON_USE_GTK_IM_MODULE=1 /data/AppImages/WeChatLinux_x86_64.AppImage
Icon=/data/AppImages/wechat.png
Type=Application
Categories=Network;Chat;
Comment=WeChat Linux版
Terminal=false
```
