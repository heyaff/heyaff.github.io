---
title: JWT认证
date: 2020-05-30 16:24:32
categories:
  - JWT
tags:
  - JWT
  - token
---

### Token认证
移动端APP或前后端分离的项目，大多是基于Token的验证。

- 通过账号&密码登陆成功，服务器生成一个token
- 服务端把该token和userid保存到数据库（或Redis）中，然后把token值返回给前端
- 客户端每次请求都带上该token，服务端根据该token查询是否合法和过期，然后去数据库中查出来userid进行使用<!--more-->


缺点：
- 验证信息存在数据库中，每次都要根据token查询userid，增加数据库开销
- token一旦被泄露，很容易跨站请求伪造

### JWT认证
JWT是token的升级，利用userId生成Token，该token可直接被用于认证。特别适用于分布式站点的单点登录（SSO）场景

JWT示例：
```
eyJ0eXAxIjoiMTIzNCIsImFsZzIiOiJhZG1pbiIsInR5cCI6IkpXVCIsImFsZyI6IkhTMjU2In0.eyJVc2VySWQiOjEyMywiVXNlck5hbWUiOiJhZG1pbiIsImV4cCI6MTU1MjI4Njc0Ni44Nzc0MDE4fQ.pEgdmFAy73walFonEm2zbxg46Oth3dlT02HR9iVzXa8

your-256-bit-secret：GQDstcKsx0NHjPOuXOYg5MbeJ1XT0uFiwDVvVBrk
```

说明：签发生成jwt需要密钥your-256-bit-secret，这个存储在服务器端，不要泄露，在不知道密钥的情况下，是不能进行解密的，secret就是用来进行jwt的签发和jwt的验证，当然这些都是在服务器端做的事情。所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端可以伪造他人id越权攻击了。

### JWT认证流程
- 用户使用账号和密码发出post请求
- server使用私钥创建一个JWT，并返回给浏览器
- 浏览器将该JWT串放在请求头的Authorization中: Authorization: Bearer <token>, 发送给server
- server对JWT进行验证
- 验证通过后返回相应的资源给浏览器
- 用户登出，client删除token，server不做处理
<img src="/images/JWT.jpg" width="20%" height="20%">

-[JWT认证和攻击界面简单总结](https://www.v4ler1an.com/2020/11/JWT/ "V4ler1an-有毒")

### 什么是Oauth
Oauth解决了User U与Server WeiXin、Server T之间的信任问题，U用户在W上已有凭证及个人数据，U又想登录T，此时T要验证U用户的身份，W将U的部分数据给T，T就允许U登录了，实现U-W-T三者之间的信任链。其中W地位最高，W提供相关认证Oauth实现，T来对接W，U仅仅是用户。
-[Oauth应用场景](https://www.kancloud.cn/kancloud/oauth_2_0/63332 "理解OAuth 2.0")

### Oauth2.0常见的授权类型流程

<!--
<center>
<div style="display:inline-block;">{% img /images/Oauth授权码流程.png 200%}</div>
<div style="display:inline-block;margin-left:10px;">{% img /images/AppSecret安全场景.jpg 200 %}</div>
</center>
-->

- 授权码
<img src="/images/Oauth授权码流程.png" width="500"/>
针对上图，细化第④⑤步，目的是禁止客户端APP存储secret。
<img src="/images/AppSecret安全场景.jpg" width="500"/>

- 简化模式
<img src="/images/Oauth简化流程.png" width="500"/>
内网SSO基于上图认证

### Q&A
-----------------
Q：Oauth2.0解决了什么问题？
A：帮你解决如何认证User用户，至于你的后端和你的客户端（前端）如何信任，是你自己解决。
A：User用户认证：除了Oauth，还有Token、JWT
-----------------
Q：自己的客户端（前端）与后端通讯如何认证，如何防止其他APP调用你的后端？
A：客户端--->服务端：AppID&AppSecret或者增加程序调用接口的复杂度，如在获取list接口之前增加token的获取等流程，或者强制用户登录才能获取接口数据
A：服务端--->服务端：推荐AppID&AppSecret
-----------------
Q：JWT和cookie-session机制有什么区别？
A：JWT不是用来替代session的，传统大型系统依然还是cookie-session
A：JWT≈Token
A：sessionID一般自定义生成`SESSIONID=254H76BA61EN0FB`，并用Redis缓存sessionID
A：JWT是无状态的，不需要服务器端保存会话信息，直接存储在客户端上localstorage

