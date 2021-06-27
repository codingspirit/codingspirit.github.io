---
title: 'Jenkins: Check Online/Offline Salve Nodes'
top: false
tags:
  - Jenkins
  - CI/CD
date: 2021-06-27 09:25:09
categories:
---
...
<!--more-->

Manage Jenkins > Script Console

```
for (node in Jenkins.instance.nodes) {
  println "${node.name}, ${node.numExecutors}"
  def computer = node.computer
  println "Online: ${computer.online}, ${computer.connectTime} (${computer.offlineCauseReason})"
}
```
