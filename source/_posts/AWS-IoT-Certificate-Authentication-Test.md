---
title: AWS IoT Certificate Authentication Test
top: false
tags:
  - IoT
date: 2022-02-27 11:57:17
categories: 随便写写
---

AWS IoT allow user to use x.509 certificate to get services authentication(like KVS) via AWS IoT core. To verify if policies are correctly attached, we can use curl to check if temporary token can be granted or not:

```bash
curl -H "x-amzn-iot-thingname:my_thing_name" --cert certificate.pem --key private.pem.key  https://xxxxxxxxxxxxxx.credentials.iot.us-east-1.amazonaws.com/role-aliases/my_thing_role_aliases/credentials --cacert cacert.pem
```
