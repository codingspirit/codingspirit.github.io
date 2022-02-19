---
title: 'Gstreamer: pipeline notes'
top: false
tags:
  - 日志
date: 2021-05-05 16:50:38
categories: 随便写写
---

...

<!--more-->

## Get raw opus data from wav file

```
gst-launch-1.0 filesrc location="input.wav" ! wavparse ! decodebin ! audioconvert ! audioresample ! opusenc ! audio/x-opus,rate=8000,channels=1 ! filesink location="output.opus"
```

## Get raw aac data from wav file

```
gst-launch-1.0 filesrc location="input.wav" ! wavparse ! decodebin ! audioconvert ! audio/x-raw,rate=8000,channels=1 ! faac ! audio/mpeg,rate=8000,channels=1,stream-format=raw ! multifilesink location="frame-%03d.aac" index=1
```

## Get raw aac data from wav file in specific chunk size

```
gst-launch-1.0 filesrc location="input.wav" ! wavparse ! decodebin ! audioconvert ! audio/x-raw,rate=8000,channels=1 ! audiobuffersplit output-buffer-duration=1/25 ! alawenc ! audio/x-alaw,rate=8000,channels=1 ! multifilesink location="frame-%03d.alaw" max-file-size=320 index=1
```

To be continued...
