---
title: SNI
date: 2020-11-30
categories:
  - SNI
tags:
  - SNI
---

- [浅谈SNI兼容性导致HTTPS出错问题](https://wwww.lvmoo.com/933.love "凯の秘密基地")
- [为什么SSL测试结果会显示两个RSA证书？](https://bbs.appnode.com/thread-877.htm "")

### SNI背景介绍
**SNI** (Server Name Indication)主要解决一台服务器只能使用一个证书(一个域名)的缺点，随着服务器对虚拟主机的支持，一个服务器上可以为多个域名提供服务。有 SNI 机制后，可以在同一个IP 下绑定多个证书，但是客户端在请求时需要先带上域名，这样服务端才能找到对应的 SSL 证书，因此SNI必须得到支持才能满足需求<!--more-->
![支持SNI的客户端获取到的server证书](/images/SNI-1.png)
![不支持SNI的客户端获取到的server证书](/images/SNI-2.png)

### 命令行测试
模拟支持SNI的浏览器获取到的证书，使用以下命令测试：
`openssl s_client -connect www.appnode.com:443 -servername www.appnode.com -showcerts`

模拟不支持SNI的浏览器获取到的证书，使用以下命令测试：
`openssl s_client -connect www.appnode.com:443 -showcerts`


### 对于不支持 SNI 的客户端
* 建议客户端升级或使用新版本的浏览器（如Chrome、Firefox等）
* 如果是微信、支付宝第三方回调，需要让其调用源站IP，绕过Web应用防火墙
