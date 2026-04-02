---
title: "Arch Linux 下 LocalSend 接收问题"
date: 2026-04-02T08:22:34+08:00
categories: ["Linux"]
tags: ["Linux", "Arch", "EndeavourOS", "Firewall", "LocalSend"]
---

LocalSend is a free, open-source app that allows you to securely share files and messages with nearby devices over your local network without needing an internet connection.
<!--more-->

在 EndeavourOS 下使用 LocalSend 的时候发现，能正常发现局域网内的其他设备进行文件传输，但是其他设备看不到本机。原因是 EndeavourOS 默认开启的防火墙规则是宽进严出的，需要增加开放 LocalSend 的端口规则才可以。

EndeavourOS 默认是使用 Firewalld 作为防火墙，使用 firewall-cmd 命令可以很方便的设置防火墙规则， LocalSend 默认使用 53317 端口，使用 TCP 和 UDP 协议。

```bash
firewall-cmd --get-active-zones  # 查看当前网口对应的区域，一般使用的区域是 public
sudo firewall-cmd --zone=public --add-port=53317/tcp  # 如果要添加永久规则需要添加 --permanent 参数
sudo firewall-cmd --zone=public --add-port=53317/udp
sudo firewall-cmd --reload  # 添加永久规则时需要重载规则，或者是重启防火墙
firewall-cmd --list-ports  # 查看当前开放的端口规则
```

完成这些规则设置，本机 LocalSend 就可以与其他设备互通了。
