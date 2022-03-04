---
title: 'Yocto: Enable USB Android Debug Bridge On Raspberry Pi'
top: false
tags:
  - Linux
  - Yocto
date: 2021-08-14 11:02:06
categories: 系统构建
---

Some RPi models(like Pi Zero and Pi 4) have OTG capabilities which allowed us to use them as USB gadgets. Since ADB can be connected via USB FFS, we are able to bring up adbd on RPi and connect to it via USB.

<!--more-->

## Enable systemd

Follow this post to replace sysvinit with systemd in your distribution:
[Yocto: Replace sysvinit with systemd](/2019/08/17/Yocto-Replace-sysvinit-with-systemd/)

## Add android-tools into build

Recipe `android-tools` in `meta-openembedded/meta-oe` provides android tools like fastboot and adb/adbd. Add it into your *bblayers.conf*:

```bash
bitbake-layers add-layer <path_to_meta-openembedded/meta-oe>
```

Then add adbd into image:

```bb
IMAGE_INSTALL_append = " android-tools-adbd"
```

To automatically setup USB FFS, use `android-tools-conf-configfs` as config provider:

```conf
# add in local.conf
PREFERRED_PROVIDER_android-tools-conf = "android-tools-conf-configfs"
```

`android-tools-adbd` will check if file `/var/usb-debugging-enabled` exists before running adbd. If you want to connect to device via USB adb once device booted up, implement a *bbapend* to install `/var/usb-debugging-enabled` to target:

*android-tools_%.bbappend*:

```bb
FILESEXTRAPATHS_prepend:="${THISDIR}/${PN}:"

SRC_URI += "file://usb-debugging-enabled"
FILES_${PN}-adbd += "${localstatedir}/usb-debugging-enabled"

do_install_append() {
   install -d ${D}${localstatedir}
   install -m 0600 ${WORKDIR}/usb-debugging-enabled ${D}${localstatedir}/usb-debugging-enabled
}

```

## Enable USB FFS support in kernel

The kernel used by meta-raspberry `linux-raspberrypi` doesn't provide support for USB FFS by default, we need to implement a *bbappend* to patch kernel config:

```
recipes-kernel
└── linux
    ├── files
    │   └── enable-ffs.cfg
    └── linux-raspberrypi_%.bbappend
```

*enable-ffs.cfg*:

```cfg
CONFIG_USB_FUNCTIONFS=m
# CONFIG_USB_FUNCTIONFS_ETH is not set
# CONFIG_USB_FUNCTIONFS_RNDIS is not set
CONFIG_USB_FUNCTIONFS_GENERIC=y
```

*linux-raspberrypi_%.bbappend*:

```
FILESEXTRAPATHS_prepend:="${THISDIR}/files:"

SRC_URI += "file://enable-ffs.cfg"
```

## Enable RPi DWC2

DWC2 is not enabled by default, we need to enable it in *machine.conf* or *local.conf*:

```conf
ENABLE_DWC2_PERIPHERAL = "1"
```

## Verify

We can verify it via `` once device is booted up. Connect the USB OTG port:

```bash
adb devices
List of devices attached
0123456789ABCDEF	device
```
