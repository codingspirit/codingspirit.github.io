---
title: Raspberry Pi RTSP server
top: false
tags:
  - 日志
date: 2021-11-07 11:51:34
categories: 随便写写
---

```bash
raspivid -o - -t 0 | cvlc -v stream:///dev/stdin --sout '#rtp{sdp=rtsp://:5554/}' :demux=h264
```
