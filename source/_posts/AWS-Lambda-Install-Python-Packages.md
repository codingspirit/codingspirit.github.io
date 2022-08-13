---
title: 'AWS Lambda: Install Python Packages'
top: false
tags:
  - Python
  - Serverless
  - Backend
date: 2022-08-12 18:24:12
categories: Cloud Service
---
...
<!--more-->
Lots of Python packages has dependencies on native C/C++ libraries. The default `pip3 install {package_name}` will install native libraries compatible with host machine, which might not work for AWS Lambda runtime.

For Lambda with architecture x86_64, using following command to install dependencies

```bash
pip3 install --platform manylinux2014_x86_64 --target={lambda_folder} --implementation cp --python 3.9 --only-binary=:all: {package_name}
```

For Lambda with architecture arm64, replace `manylinux2014_x86_64` with `manylinux2014_aarch64`.
