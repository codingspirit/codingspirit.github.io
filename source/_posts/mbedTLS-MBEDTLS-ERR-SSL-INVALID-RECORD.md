---
title: 'mbedTLS: MBEDTLS_ERR_SSL_INVALID_RECORD'
top: false
tags:
  - mbedTLS
date: 2021-02-27 08:58:00
categories: 问题记录
---

Using some RTOS built-in mbedTLS to connect to AWS IoT Core might fail due to `MBEDTLS_ERR_SSL_INVALID_RECORD`(-0x7200 or -29184) during ssl handshake. There are several possible causes:

- The task stack set for mbedTLS running is not enough. Increase task stack and try again.
- `MBEDTLS_SSL_MAX_CONTENT_LEN` is not enough. Increase `MBEDTLS_SSL_MAX_CONTENT_LEN` to 16384 and try again.
