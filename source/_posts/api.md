---
title: API&签名
date: 2020-10-17 19:12:57
categories:
  - 签名
tags:
  - 接口
---


### 两种签名
![](/images/sign.png )
① 双方共享/知道某个secret，用同一个算法来签名/验签（如对称密钥HMAC算法）
② A用私钥签名，B用A的公钥验签
<!--more-->
前提条件
**验签方B**需要提前知道**签名方A**的公钥或APPsecret
签名是B用来验证A的身份，同时保证A传的数据防篡改


### 接口安全
如果是前后端分离的系统，那么接口一定会暴露在js中，多找找可能有未授权的惊喜

### 流量分析识别接口特征
核心问题：怎么判断流量中是不是接口？
衍生问题：app请求是api吗？http请求都是api吗？js/css/html静态文件是api吗？
[敏感数据保护的基石：API资产发现与管理](https://www.aqniu.com/industry/85974.html "网宿安全")

### API资产管理系统
1、如果流量很大，那就抽样或针对部分重点业务采集流量分析，或导accesslog分析
2、旁路在交换机的流量分析API画像，如果body是加密的，是无法分析的
3、落地的难度，怎么去推这个产品，再者各种api的授权机制太复杂
4、业务风控的主线是账号安全，API的主线是API信息
5、优点是发现僵尸接口，缺陷接口
6、东西向流量不适合部署WAF、可以部署API网关


### Q&A
---------------
Q：签名是在哪做？是为了啥？证明谁的身份？场景是什么？
A：签名的一方发起请求，对方进行验签。目的是为了验证签名那一方的身份及数据没有被篡改
A：签名在 *请求/调用方* 那一侧做，当然 *接收/被调* 那一方相应的要做验签，双方提前要沟通好算法保持一致
---------------
Q：客户端请求服务器端，API请求怎么签名？
1、客户端内置APPkey和secret，sign字段=MD5(key1value1key2value2...secret)进行MD5计算发给服务端
2、建议把APPsecret放到NDK中编译成so文件，app启动后去读取
3、或者第一次APP安装启动时，下发secret
---------------
Q：浏览器请求服务器端，API请求怎么签名？
A：js中保存APPsecret有较高风险，建议采用随机分配令牌的方式，为每个访问端分配一个token保存在cookie中，通过cookie请求头(cookie:h5_token=xxx;sessionid=xxx)传回服务端，sign=MD5(key1value1key2value2...token)
A：- [淘宝sign签名算法](https://mp.weixin.qq.com/s?__biz=MzIwNjUxMTQyMA%3D%3D&chksm=9721cf1ca056460a644f61bb84c255eb7620ac1905e8652ffb1a869c5432aac77a81400f90bd&idx=1&lang=zh_CN&mid=2247484239&scene=21&sn=1530557bc16ad40616d6614017a872d7&token=205497283#wechat_redirect "小歪丶 Python爬虫与算法进阶")
---------------
Q：APP签名机制？
A：开发者把自己的公钥上传到苹果，苹果给每个开发者颁发证书，在IOS上安装APP时，苹果会校验证书签名
---------------

[第三方应用安全开发指南](https://opendocs.alipay.com/open/common/105912 "支付宝应用安全开发指南")

<!--<object data="/pdf/开放平台第三方应用安全开发指南.pdf" type="application/pdf" width="100px" height="677px">-->

