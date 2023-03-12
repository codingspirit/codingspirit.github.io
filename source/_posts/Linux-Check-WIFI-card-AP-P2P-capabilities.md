---
title: 'Linux: Check WIFI card AP/P2P capabilities'
top: false
tags:
  - Linux
date: 2023-03-12 09:12:30
categories: 系统构建
---
...
<!--more-->
```bash
iw list | grep "Supported interface modes" -A 8
Supported interface modes:
          * IBSS
          * managed
          * AP
          * AP/VLAN
          * WDS
          * monitor
          * P2P-client
          * P2P-GO
```
