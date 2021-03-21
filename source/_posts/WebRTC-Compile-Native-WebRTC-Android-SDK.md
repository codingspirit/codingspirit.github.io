---
title: 'WebRTC: Compile Native WebRTC Android SDK'
top: false
tags:
  - WebRTC
  - Android
date: 2021-03-21 08:22:56
categories: 编程相关
---

The very first step of modifying native WebRTC SDK for Android is clone source code from Google repository and compile it into jar.


<!--more-->

## Install depot_tools
**depot_tools** is a set of version control/build tools. We can get it from chromium repository:

```bash
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

Then export those tools into `PATH`:

```bash
export PATH=$PATH:/${depot_tools_path}
```

## Fetch code

```bash
cd ${path_you_want_to_save_webrtc_code}
fetch --nohooks webrtc_android
gclient sync
```

## Checkout to your base branch

I was using `m79` :

```bash
cd webrtc_android/src/
git checkout branch-heads/m79
```


## Install build dependencies and compile

```bash
cd webrtc_android/src/
./build/install-build-deps.sh
./tools_webrtc/android/build_aar.py
```

**build_aar.py** will compile code, and package native and jar libraries into *webrtc_android/src/libwebrtc.aar*

## Use the built libwebrtc.aar in your Android Studio app project

1. Copy aar files to `${your_android_studio_app_project}/libs/`
2. If you have `implementation 'org.webrtc:google-webrtc:xxxxxxxxxx'` in `build.gradle`, remove it.




