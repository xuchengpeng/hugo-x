---
title: "打包百度网盘 AppImage"
date: 2026-03-27T15:44:23+08:00
categories: ["Linux"]
tags: ["Linux", "Arch", "AppImage"]
---

百度网盘官方只提供了 Linux 下的 deb 和 rpm 格式包文件，在 Arch 或者 SUSE 等发行版上使用不太方便。
<!--more-->

把它打包成 AppImage 格式，这样基本上就可以在所有的 Linux 发行版上方便的使用了。

打包需要使用 appimagetool 工具，下载后直接就可以使用：

```bash
mkdir build
cd build
wget https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage -O appimagetool
chmod a+x ./appimagetool
```

然后下载官方的 deb 包并解压到需要打包的目录：

```bash
mkdir $APP.AppDir
wget https://issuepcdn.baidupcs.com/issue/netdisk/LinuxGuanjia/4.17.8/baidunetdisk_4.17.8_amd64.deb
ar x ./baidunetdisk_4.17.8_amd64.deb
tar xf ./data.tar.bz2 -C $APP.AppDir
```

在打包前，需要先创建 AppRun、desktop 和图标：

```bash
cd $APP.AppDir
cat >> ./AppRun << 'EOF'
#!/bin/sh
HERE="$(dirname "$(readlink -f "${0}")")"
exec "${HERE}"/opt/baidunetdisk/baidunetdisk --no-sandbox "$@"
EOF
chmod a+x ./AppRun
ln -s usr/share/applications/baidunetdisk.desktop baidunetdisk.desktop
ln -s usr/share/icons/hicolor/scalable/apps/baidunetdisk.svg baidunetdisk.svg
cd ../
```

最后，使用 appimagetool 进行打包：

```bash
ARCH=x86_64 ./appimagetool -n --verbose ./$APP.AppDir ../$APP-$VER-x86_64.AppImage
```

整个打包过程脚本，我放在了 [baidunetdisk-appimage](https://github.com/xuchengpeng/baidunetdisk-appimage) 仓库，使用了 Github Actions 自动打包并发布了版本。
