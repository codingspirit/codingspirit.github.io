---
title: 'AWS Lambda: Python logging'
top: false
tags:
  - Python
  - Serverless
  - Backend
date: 2022-07-30 18:22:48
categories: Cloud Service
---

<!--more-->

# Log to local Python interpreter when test locally

```py
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()
logger.info('some log)
```

But this will not log to Cloud Watch when test on the cloud.

# Log to Cloud Watch

AWS Lambda Python runtime has pre-configured log handlers. To log to Cloud Watch:

```py
logger = logging.getLogger()
logger.setLevel(logging.INFO)
logger.info('some log)
```

But this will not log to local Python interpreter when test locally.

# Log to both Cloud Watch and local

```py
# If there is no pre-configured log handler, we set basicConfig
if not logging.getLogger().hasHandlers():
  logging.basicConfig(level=logging.INFO)

logger = logging.getLogger()
logger.setLevel(logging.INFO)
```
