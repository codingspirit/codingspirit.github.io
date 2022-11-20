---
title: 'Java: JSON to Map'
top: false
tags:
  - Java
date: 2022-11-19 18:58:39
categories: 编程相关
---

We can construct a `Map<String, Object>` from a JSON string with the help of reflect:


```java
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

 var mapType = new TypeToken<Map<String, Object>>() {
 }.getType();
 Map<String, Object> myMap = new Gson().fromJson(jsonString, mapType);
```
