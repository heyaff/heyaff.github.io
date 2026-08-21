---
title: 浏览器同源限制，CORS实现跨域请求
date: 2020-03-04 19:12:57
categories:
  - CORS
tags:
  - 跨域
  - CORS
---

-[跨域踩坑经验总结（内涵：跨域知识科普）](https://ningyu1.github.io/site/post/92-cors-ajax/ "凝雨")
-[跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html "阮一峰的网络日志")


### 什么情况下出现跨域

![](/images/cors.png )
浏览器有同源策略限制
前后端数据交互经常会碰到请求跨域
如果缺少了同源策略，浏览器很容易受到XSS、CSRF等攻击
**例外**
同源策略是浏览器需要遵循的标准，而如果是服务器向服务器的请求（后端-->后端）就无需遵循同源策略
对于`<a>` `<script>` `<img>` `<video>` `<link>`这类属性带有src、href的标签，允许跨域加载
<!--more-->

### 跨域请求方式
#### 1、JSONP（废弃）
只支持GET请求（不支持POST），安全性差，不推荐

#### 2、CORS W3C标准 跨域资源共享  
CORS需要服务器设置header `Access-Control-Allow-Origin` 白名单等等，安全性高，**推荐使用**
+ 2.1 对于简单请求：
浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个Origin字段。
```
GET /cors HTTP/1.1
Origin: http://www.cmft.com
Host: api.cmft.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

+ 2.2 对于复杂请求：
非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求OPTIONS
```
OPTIONS /cors HTTP/1.1
Origin: http://www.cmft.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.cmft.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```
用来询问HOST，关键字段是Origin，表示请求来自哪个源。RESPONSE只有header，没有响应包。
整个CORS通信过程，都是浏览器自动完成，不需要用户参与
![](/images/response.png)

<font color=#DC143C>【Tips】即使设置了`Access-Control-Allow-Origin`，但CORS只对浏览器起作用，对curl或postman或普通程序不起作用，都是可访问的。</font>


#### 3、Nginx反向代理
Nginx中转服务器，用于转发请求（NG与其他服务端之间的资源请求不会有跨域限制）

例如`www.123.com/index.html`需要调用`www.456.com/server.php`，可以写一个接口`www.123.com/api/server.php`，由这个接口在后端去调用`www.456.com/server.php`并拿到返回值，然后再返回给index.html。用nginx把/api路径转发到其他域名那里，相当于绕过了浏览器端，自然就不存在跨域问题。


### CORS前端报错

报错一：
```
The value of the 'Access-Control-Allow-Credentials' header in the response is '' which must be 'true' when the request's credentials mode is 'include'. 
```
解决方法：
因为请求头中有设置cookie，如登录认证。所以response响应头必须添加`Access-Control-Allow-Credentials: true`

报错二：
```
The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'. 
```
解决方法：
响应头`Access-Control-Allow-Origin: *`中的*改为具体的https://域名
