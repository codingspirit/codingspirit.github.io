---
title: Install AWS CLI V2 On Raspberry Pi
top: false
tags:
  - 日志
date: 2021-11-02 05:32:00
categories: 随便写写
---

AWS doesn't provide official AWS CLI V2 binary release for Raspberry Pi. To install V2 on Raspberry Pi, we have to install it from source:

```bash
git clone https://github.com/aws/aws-cli.git
cd aws-cli && git checkout v2
sudo pip3 install -r requirements.txt
sudo pip3 install . --prefix /usr/local
```
