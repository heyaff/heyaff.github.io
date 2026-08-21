---
title: session和cookie
date: 2020-08-21 19:12:57
categories:
  - session
tags:
  - session
---

- [token在哪里](https://blog.lishunyang.com/2021/09/where-is-token.html "Lishunyang's Blog")
 
### session和cookie
![session-cookie](/images/session.png)
1、session-cookie是一对，session保存在服务端memcache/redis，cookie保存客户端硬盘

2、客户端只需要保存session_id，加密存储在cookie硬盘中，发送给服务端。<!--more-->

3、浏览器把cookie以KV形式存储到硬盘中。session等登录用户信息存储在服务端

4、API模式是不支持cookie的

5、session_id欺骗`Cookie: JSESSIONID=2CA28F5D92F7CF0DFB1B21CBD3F161B9`

### token
1、token是为了解决session存储问题的

2、token是在服务端用私钥签名生成的，发送给客户端后，客户端保存在cookie/localStorage（本地存储），在服务端验证token，可存储也可不存（session必须存储）。

3、移动端对cookie的支持不是很好，而session需要基于cookie实现，所以移动端常用的是token

### HTTP请求token的位置
#### 放在请求URI中
- 不安全，容易被盗用（复制链接给他人），放在URI中仅限有限次数使用、或只具有受限的权限、临时登录等场景。如OAuth2用一次就失效的token就叫code

#### 放在请求header中
- 放在自定义header里
	+ 推荐
	+ 需前端代码手动设置自定义header（当然用户认证的过程还是后端代码实现）
	   - 此方案跟浏览器没任何关系，token怎么获取/怎么注入到请求头/怎么管理有效期，全部由自己写代码实现，不受浏览器限制
- 放在cookie头里
	+ 仅限PC浏览器环境，移动APP是没有cookie头的
	+ 依赖浏览器环境，APP原生应用或小程序需开发者自己去搞cookie
	+ cookie的各种操作由浏览器来控制，cookie又是后端通过`set-cookie`种上去的，所以是后端代码控制，前端可以不写任何代码

#### 放在body中

- GET请求咋办？所以基本无！

