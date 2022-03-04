---
title: 'WebRTC: H.264 On Non-Qualcomm Android Devices'
top: false
tags:
  - WebRTC
  - Android
date: 2021-07-11 09:18:16
categories: 编程相关
---

User might expiring SDP failure on non-Qualcomm/SAMSUNG chip Android devices when received SDP from another peer that only support H.264. This was caused by a limitation of Android WebRTC SDK that it will check H.264 capabilities via a chip whitelist, which only has Qualcomm and Exynos listed. To make it available for other Android devices, we need to modify and recompile WebRTC Android SDK.

<!--more-->

## Fetch Code And Test Build

[WebRTC: Compile Native WebRTC Android SDK](/2021/03/21/WebRTC-Compile-Native-WebRTC-Android-SDK/)

## Modifications

Original code in *HardwareVideoEncoderFactory.java*:

```java
private boolean isHardwareSupportedInCurrentSdkH264(MediaCodecInfo info) {
  // First, H264 hardware might perform poorly on this model.
  if (H264_HW_EXCEPTION_MODELS.contains(Build.MODEL)) {
    return false;
  }
  String name = info.getName();
  // QCOM H264 encoder is supported in KITKAT or later.
  return (name.startsWith(QCOM_PREFIX) && Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT)
      // Exynos H264 encoder is supported in LOLLIPOP or later.
      || (name.startsWith(EXYNOS_PREFIX)
              && Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP);
}
```

You could add your chips into whitelist here, but it might be easier to remove whitelist check if your target devices is running Android 5.0+(LOLLIPOP), since basically all chips in Android devices support H.264 nowadays.

```java
private boolean isHardwareSupportedInCurrentSdkH264(MediaCodecInfo info) {
  // First, H264 hardware might perform poorly on this model.
  if (H264_HW_EXCEPTION_MODELS.contains(Build.MODEL)) {
    return false;
  }
  return (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP);
}
```
Same modifications should be added into *MediaCodecVideoDecoderFactory.java* as well.
Original code in *MediaCodecVideoDecoderFactory.java*:

```java
private boolean isH264HighProfileSupported(MediaCodecInfo info) {
  String name = info.getName();
  // Support H.264 HP decoding on QCOM chips for Android L and above.
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP && name.startsWith(QCOM_PREFIX)) {
    return true;
  }
  // Support H.264 HP decoding on Exynos chips for Android M and above.
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M && name.startsWith(EXYNOS_PREFIX)) {
    return true;
  }
  return false;
}
```

modified:

```java
private boolean isH264HighProfileSupported(MediaCodecInfo info) {
  return (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP);
}
```

Then recompile SDK by following [Fetch Code And Test Build](#Fetch-Code-And-Test-Build).
