---
title: "使用 FFmpeg 裁剪视频"
date: 2026-03-26T09:24:17+08:00
categories: ["Linux"]
tags: ["Linux", "FFmpeg"]
---

FFmpeg is complete, cross-platform solution to record, convert and stream audio and video.
<!--more-->

日常生活和工作中，经常会有转换视频格式，或者是截取长视频中某一段的诉求， FFmpeg 是其中功能最强大的一个，很多其他的视频工具其实都用到了它。

比如，截取一个 10s 视频的前 3s 保存成一个新文件，可以精确到毫秒。

```bash
ffmpeg -i input_10s.mp4 -ss 00:00:00.000 -to 00:00:03.000 output_3s.mp4
```
