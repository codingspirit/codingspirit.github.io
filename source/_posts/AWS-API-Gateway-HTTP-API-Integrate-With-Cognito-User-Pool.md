---
title: 'AWS API Gateway: HTTP API Integrate With Cognito User Pool'
top: false
tags:
  - Backend
date: 2022-12-10 18:00:29
categories: Cloud Service
---
...
<!--more-->

1. Name of the authorizer -> Cognito User Pool App client **name**
   - AWS Cognito -> User pools -> App Integration -> App client settings > App client
2. Identity Source -> `$request.header.Authorization`
3. Issuer URL -> Cognito idp url
   - https://cognito-idp.{aws_region}.amazonaws.com/{user_pool_id}
4. Audience -> Cognito User Pool App client **id**
   - AWS Cognito -> User pools -> App Integration -> App client settings > App clientID
