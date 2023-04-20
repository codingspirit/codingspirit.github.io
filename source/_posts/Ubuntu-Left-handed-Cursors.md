---
title: 'Ubuntu: Left-handed Cursors'
top: false
tags:
  - 日志
date: 2023-04-02 09:44:00
categories: 随便写写
---

<!--more-->

Ubuntu 22.04 is using **Yaru** theme by default, which provided a left-handed cursor already. There is no need to install extra tools/themes to make cursor left-handed.

```bash
cd /usr/share/icons/Yaru/cursors
sudo mv left_ptr left_ptr_origin
sudo mv right_ptr left_ptr
```

After rebooting, left-handed cursor will be applied. To switch primary button on mice:

Settings -> Mouse & Touchpad -> General -> Primary Button, Select **Right**
