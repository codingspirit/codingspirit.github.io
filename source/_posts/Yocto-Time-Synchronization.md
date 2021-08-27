---
title: 'Yocto: Time Synchronization'
top: false
tags:
  - Yocto
  - Linux
date: 2021-08-27 18:24:14
categories: 系统构建
---

...

<!--more-->

## timedatectl

`timedatectl` will enable ntp by default and automatically synchronize system time if `systemd` was chosen as system init service. The recipe provides by openembedded will use google's ntp server in configurations, user might need to append `systemd` recipe to make it work in China mainland.

```
recipes-core/systemd
├── systemd
│   └── timesyncd.conf
└── systemd_%.bbappend

```

Here we use ntp server provided by ubuntu in *timesyncd.conf*:

```conf
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See timesyncd.conf(5) for details.

[Time]
NTP=ntp.ubuntu.com
```

*systemd_%.bbappend*:

```bb
FILESEXTRAPATHS_prepend:="${THISDIR}/${PN}:"

SRC_URI += "file://timesyncd.conf"

do_install_append() {
    install -m 0644 ${WORKDIR}/timesyncd.conf ${D}${sysconfdir}/systemd
}

```

## ntpd

If `systemd` was disabled, user need to enable `ntpd` support in `busybox`.


```
recipes-core/busybox/
├── busybox
│   ├── enable-ntpd.cfg
│   └── ntp.conf
└── busybox_%.bbappend
```

*enable-ntpd.cfg*:

```
CONFIG_NTPD=y
CONFIG_FEATURE_NTPD_SERVER=y
CONFIG_FEATURE_NTPD_CONF=y
CONFIG_FEATURE_NTP_AUTH=y
```

In *ntp.conf*, we use ntp server pool:

```conf
# This is the most basic ntp configuration file
# The driftfile must remain in a place specific to this
# machine - it records the machine specific clock error
driftfile /etc/ntp.drift
# This obtains a random server which will be close
# (in IP terms) to the machine.  Add other servers
# as required, or change this.
server pool.ntp.org
# Using local hardware clock as fallback
server 127.127.1.0
fudge 127.127.1.0 stratum 14
# Defining a default security setting
restrict default nomodify nopeer
```

*busybox_%.bbappend*:

```bb
FILESEXTRAPATHS_prepend:="${THISDIR}/${PN}:"

#enable ntpd
SRC_URI += "file://enable-ntpd.cfg \
            file://ntp.conf"
FILES_${PN} += "${sysconfdir}/ntp.conf"

do_install_append() {
    install -d ${D}${sysconfdir}
    install -m 0644 ${WORKDIR}/ntp.conf ${D}${sysconfdir}/ntp.conf
}

```

To trigger time synchronization while booting up, user need to execute `ntpd -gq` in boot scripts. This will make `ntpd` run only once, synchronize system time and quit, which might be useful for saving system resources.
