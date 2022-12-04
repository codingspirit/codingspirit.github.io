---
title: 'AWS CDK: attach condition to CDK resource'
top: false
tags:
  - Backend
  - Java
  - CDK
date: 2022-12-03 19:09:52
categories: Cloud Service
---

AWS Cloudformation allows user attach condition to resource, so resource will be created only if the given condition has been fulfilled(conditional resource). But things get tricky when using AWS CDK, user must use `getNode` method to get the `Cfnxxxxx` resource from original CDK resource and attach conditions afterwards. Take S3 Bucket as an example:

```java
((CfnBucket)myBucket.getNode().getDefaultChild()).getCfnOptions().setCondition(
         myCondition
 );
```
