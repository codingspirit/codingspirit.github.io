---
title: 'Bitbake Error: Unable to connect to bitbake server, or start one'
top: false
tags:
  - Linux
  - Yocto
date: 2022-06-24 17:06:31
categories: 问题记录
---

<!--more-->

ERROR: Unable to connect to bitbake server, or start one (server startup failures would be in bitbake-cookerdaemon.log).


This issue might be encountered if bitbake server was unexpectedly closed. Bitbake will crate a lock file in build folder, if bitbake was shut down before remove this lock file, user can't restart bitbake again. Solution is quite simple that just remove the lock file in the build folder:

```bash
rm bitbake.lock
```
