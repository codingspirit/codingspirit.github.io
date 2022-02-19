---
title: 'Go: Error When "go get"'
top: false
tags:
  - Go
date: 2022-02-19 16:08:38
categories: 问题记录
---

...

<!--more-->

When use `go get` with VPN, user might get the following errors:


```
x509: certificate is valid for xxxxxxxxxxxxxxxxxxxxxx, not proxy.golang.org
```

To resolve this error, we can set `GOPROXY` to `direct` mode:

```
go env -w GOPROXY=direct
```
