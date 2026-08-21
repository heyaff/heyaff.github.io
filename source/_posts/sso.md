---
title: sso
categories:
  - sso
tags:
  - sso
date: 2019-11-28 19:01:38
---


![](/images/sso.png)<!--more-->

### 鉴权:黑猫/UM（统一模板，控制子菜单权限）

### SSO:统一认证系统/单点登录
实现效果：访问A系统，右上角点击登录，跳转到SSO认证页面，认证通过后，返回A系统。又访问B系统，无须登录可访问B。
![SSO流程图](/images/sso1.jpg)
8、应用A拿到user的Token，要发给SSO去校验，不校验直接返回页面给user，是不安全的。校验Token是SSO的活，因为是它生成的Token
9、SSO校验Token是否真实，返回True/False给应用A。应用A将用户信息存在session中，当用户下次访问应用A时，应用A检测session中是否存在，如果存在将不会再访问SSO



### AD：域账号（存储用户的账号/密码）
若C系统不接SSO，直接用AD登录C系统自带的登陆页面，登录成功后，访问D系统，依然还得登录= =!
