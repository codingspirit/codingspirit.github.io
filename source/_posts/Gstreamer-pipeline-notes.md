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

To be continued...
