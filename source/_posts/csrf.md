---
title: CSRF和XSS
date: 2020-09-07 19:12:57
categories:
  - CSRF
tags:
  - XSS
  - CSRF
  - CORS
---


-[前端安全系列（二）：如何防止CSRF攻击？](https://tech.meituan.com/2018/10/11/fe-security-csrf.html)
### CSRF
跨站请求伪造
* #### 场景
用户访问有CSRF漏洞的网站A，然后又访问了网站B（恶意网站），网站B劫持网站A的Cookie发送请求，在用户无感知的情况下以他的名义发送请求给网站A。<!--more-->
* #### 特点
攻击者看不到Cookie内容，由于同源策略限制，也拿不到Cookie，仅仅是借助用户的Cookie发送请求给CSRF漏洞网站，也获取不到Response的内容。换句话说，GET基本上没有CSRF漏洞（因为其他操作都用POST），所以GET请求基本上不需要CSRF防护。
* #### 验证
抓包后，去掉请求头的Referer字段重新提交，若提交还有效，证明存在CSRF漏洞


* #### 预防
1、增加验证码
2、检查Referer
3、添加CSRFtoken
（CSRF攻击之所以能成功，是因为攻击者可完全伪造用户的请求，该请求中的验证信息都是存在Cookie中，因此可直接利用用户自己的Cookie来发请求。要抵御CSRF，关键在于在请求中放入攻击者不能伪造的信息，并且该信息不存在Cookie中。如GET请求csrftoken附在URL之后，POST请求在form表单最后加上`<input type="hidden" name="csrftoken" value="tokenvalue"/>`）
* ##### TIPs
在burp中，右键Engagement tools --> Generate CSRF Poc，产生一个恶意的网站B，修改value的参数值，来测试value修改后是否生效。


### XSS
跨站脚本攻击
* #### 场景
获取用户在该网站的Cookie
* #### 验证
插入`<script>alert(document.cookie)</script>`

* #### 预防
1、设置HttpOnly，禁止js读取Cookie。HttpOnly并非阻止XSS攻击，而是能阻止XSS攻击后的Cookie劫持攻击。
2、输入检查，过滤、转义
3、输出检查



### CORS漏洞
跨域资源共享漏洞
* #### 场景
获取用户的Response等敏感信息，如GET请求
* #### 验证
添加/修改请求头中Origin: `http://evil.com`字段，若返回头是Access-Control-Allow-Origin: `http://evil.com`，证明存在CORS漏洞

* #### 预防
1、如无必要，不要开启CORS
2、严格校验Access-Control-Allow-Origin白名单，禁止*
3、尽量避免使用Access-Control-Allow-Credentials





