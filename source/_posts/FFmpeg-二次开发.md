---
title: FFmpeg 二次开发
categories: [技术]
tags:
  - 音视频
  - FFmpeg
date: 2021-10-30 14:41:11
---

### 一、FFmpeg
##### 1. 代码结构
- libacvdec
  - 提供了一系列编码器的实现
  - 如果需要提供自己的编码，在此基础上进行二次开发
- libavformat
  - 实现在流协议、容器格式以及基本的IO访问
- libavutil
  - 提供了Hash器，解码器和各种工具函数
- libavfilter
  - 提供各种音视频过滤器
- libavdevice
  - 提供捕获设别的接口
- libswresample
  - 实现混音和重采样
- libswscale
  - 实现了色彩转换和缩放功能

<!--more-->

### 总结
内容