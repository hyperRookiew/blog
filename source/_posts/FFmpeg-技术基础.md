---
title: FFmpeg 技术基础
categories: [技术]
tags:
  - 音视频
  - FFmpeg
date: 2021-10-27 23:26:12
---

### 番外
格式工厂居然被FFmpeg拉黑名单了，因为没有遵守开源协议，转身就把它卸载了。

### 一、FFmpeg基础
##### 1. FFmpeg处理音视频的流程
- 解复用(demuxer)：带封装格式的输入文件->编码数据包（音频，视频，字幕等）
- 解码(decoder)：编码数据包->解码后的数据帧
- 处理（processer）：对数据帧进行处理，比如转码、转分辨率、转帧率、加滤镜等
- 编码（encoder）：解码数据帧->编码数据包
- 复用（muxer）：编码数据包->输出带封装格式的文件
- 注意：
  - 对于转封装这样的操作，不需要进行处理操作，所以不需要进行解码和编码操作，demuxer之后直接muxer就可以。

<!--more-->

### 常用FFmpeg命令
##### 1. 基本信息查询类

```
-version
-devices:显示可用设备
-formats：支持的文件
-muxer -demuxer：支持的封装和解封装
-codecs -decoders：编解码器
-filters：支持的所有过滤器
```

##### 2. FFmpeg录制命令

```
// MAC 平台录制视频
ffmpeg -f avfoundation -i 1 -r 30 out.yuv
// 参数用法
-f // 格式，采用mac提供的avfoundation库来采集视频
-i // 指定从哪里采集数据，1为屏幕设备的索引值，屏幕为1，摄像头为0
-r // 指定帧率，这里为30帧
// 播放录制的视频
ffplay -video_size 2560x1600 -pix_fmt yuv420p out.yuv
// 录制声音
ffmpeg -f avfoundation -i :1 out.wav
// 播放录制的声音
ffplay out.wav
```
##### 3. FFmpeg 分解与复用处理（媒体格式转换）

- 一般的封装格式转换转换，比如.mp4转.flv

```
// mp4转flv,这个的处理速度会非常的快
ffmpeg -i input.mp4 -vcodec copy -acodec copy out.flv
//参数介绍
-vcodec copy // 视频编解码直接拷贝，不做更改
-acodec copy // 音频编解码也不做更改
```
- 从视频中仅抽取视频或者音频

```
// 抽取已有视频流和音频流
ffmpeg -i input.mp4 -an -vcodec copy out.h254
ffmpeg -i input.mp4 -vn -acodec copy out.aac // 格式需要和视频中音频流的格式相同
// 参数介绍
-an: audio no,不要音频
-vn: video no,不要视频
```

##### 4. FFmpeg处理原始数据命令
- 原始数据
  - 视频原始数据：YUV数据
  - 音频原视数据：PCM数据
- 原始数据的提取是之后对音视频进行进一步处理的基础
- 提取YUV数据
  - 这个过程会有些长，因为涉及到编解码了

```
// 提取YUV数据
ffmpeg -i input.mp4 -an -c:v rawvideo -pix_fmt yuv420p out.yuv
// 参数意义
-c:v rawvideo     // 指定视频的编码器，选择rawvideo编解码器，得到YUV
-pix_fmt yuv420p  // 指定像素格式，这里一般指定为yuv420p （YUV4：2：0格式）
// 播放YUV数据
ffplay -video_size 2560x1600 -pix_fmt yuv420p out.yuv
```
- 提取PCM音频原视数据

```
// 提取PCM
ffmpeg -i input.mp4 -vn -ar 44100 -ac 2 -f s16le out.pcm
// 参数意义
-ar 44100   // 设置音频采样率，常用的采样率，44.1kHZ，48kHZ，32kHZ，16kHZ
-ac 2       // 设置声道
-f s16le    // 指定pcm数据的存储格式
// 播放PCM
ffplay -ar 44100 -ac 2 -f s16le out.pcm 
```
##### 5. FFmpeg滤镜命令
- 基于avfilter组件，多用于多媒体的处理与编辑
- 可以实现加水印，去水印，画中画，裁剪，倍速播放
- 滤镜的工作原理
  - 对解码后的数据帧再进行过滤（处理）
  - 对过滤后的帧再进行编码
- 视频裁剪命令

```
ffmpeg -i input.mp4 -vf crop=in_w-200:in_h-200 -c:v libx264 -c:a copy out
// 参数使用
-vf crop=in_w-200:in_h-200  // 指定滤镜名字为裁剪，之后为该滤镜的参数，宽高为200
-c:v libx264                // 指定视频处理的编码器
-c:a copy                   // 音频不做处理
```
- 视频加水印命令

```
// movie滤镜，添加水印
ffmpeg -i input.flv -preset veryslow -c:v libx264 -c:a copy -vf "movie=pic.png[wm];[in][wm] overlay=100:100[out]" overlay.flv
// 参数使用
-preset veryslow  // 慢模式，最终视频质量损失较低
```

##### 6. FFmpeg裁剪与合并命令
- 视频裁剪命令

```
// 基于开始时间和时长进行裁剪
ffmpeg -i input.flv -ss 00:00:00 -t 10 out.ts
// 参数使用
-ss 00:00:00  // 指定开始裁剪的时间，时：分：秒
-t  10        // 裁剪的时长，以秒为单位
```

- 视频拼接命令

```
// 视频拼接命令
ffmpeg -f concat -i input.txt out.flv
// 参数
-f concat       // 指定为拼接
-i inputs.txt   //指定视频列表的文件
// input.txt文件内容
file '1.ts'
file '2.ts'
```

##### 7. 图片/视频互转
- 视频转图片

```
// 视频转多张图片序列
ffmpeg -i input.flv -r 1 -f image2 image-%3d.jpeg
// 参数使用
-r 1    // 指定转换图片的频率，每秒转出一张
-f image2 //  指定图片的输出格式
image-%3d.jepg  // 文件名以image-加三个动态数字
```

- 多张图片序列转视频

```
// 图片转视频
ffmpeg -i image-%3d.jpeg -r 1 out.mp4
```

##### 8. 直播相关的命令
- 直播推流

```
ffmpeg -re -i input.mp4 -c copy -f flv rtmp://ip:1935/live/stream_name
//参数
-re   // 减慢推流帧率速度, 如果不加的话，ffmpeg会把所有数据一股脑的把所有的数据推出去。本来一个2分钟的视频 他可能几秒钟就推完了
-f flv 需要指定输出的格式为rtmp
```
```
ffmpeg -i rtmp://ip:1935/live/stream_name -c copy video.flv
```


### 总结
内容