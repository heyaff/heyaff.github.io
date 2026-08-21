---
title: HTTP/2 & QUIC 协议
date: 2021-01-08
categories:
  - QUIC
tags:
  - HTTP/2
---

### HTTP/2对比HTTP 1.1
HTTP/2也被称为HTTP 2.0，相对于HTTP 1.1的新增多路复用、压缩HTTP头、划分请求优先级、服务端推送等特性，解决了在HTTP 1.1中一直存在的问题，优化了请求性能，同时兼容了HTTP 1.1的语义。<!--more-->

### HTTP/2优势

- 二进制协议：相比于HTTP 1.x基于文本的解析，HTTP/2将所有的传输信息分割为更小的消息和帧，并对它们采用二进制格式编码。基于二进制可以使协议有更多的扩展性，例如，引入帧来传输数据和指令。
- 内容安全：HTTP/2基于HTTPS，具有安全特性。使用HTTP/2特性可以避免单纯使用HTTPS引起的性能下降问题。
- 多路复用（MultiPlexing）：通过该功能，在一条连接上，您的浏览器可以同时发起无数个请求，并且响应可以同时返回。另外，多路复用中支持了流的优先级（Stream dependencies）设置，允许客户端告知服务器最优资源，可以优先传输。
- Header压缩（Header compression）：HTTP请求头带有大量信息，而且每次都要重复发送。HTTP/2采用HPACK格式进行压缩传输，通讯双方各自缓存一份头域索引表，相同的消息头只发送索引号，从而提高效率和速度。


### HTTP的访问请求只能是HTTP/1.1，HTTPS下才会有H2

### Nginx支持H2配置，但不支持后端应用也启用了H2
```
server {
　　 listen 80;
　　 listen [::]:80;
　　 listen 443 ssl http2;
　　 listen [::]:443 ssl http2;
　　 server_name example.com;
```

### 为啥抓包工具抓不到HTTP/2
BURP_2020.6版本之后支持HTTP/2


### QUIC基于UDP协议，属于传输层
- 1RTT/0RTT
- QUIC的1个RTT就是用来获取服务端的公钥，缓存到客户端，后续0-RTT
- TLS1.3传输层安全协议
- 连接迁移，使用UUID标识每一次连接，当手机切换网络，自身IP变化的场景，对于TCP连接（源目IP+源目端口）就会失效，需重新建立连接。但只要UUID不变，QUIC就不需要重新握手，继续传输数据

### QUIC缺点
- 推广慢，运营商/公司一般会对53端口（DNS）以外的UDP流量拦截或限流
- UDP放大攻击，攻击者控制肉机向受害者投放大量的UDP流量


### HTTP/3，基于QUIC协议的HTTP（HTTP-over-QUIC）命名为HTTP/3
QUIC协议不止用于HTTP，也可用于其他


### 客户端开启HTTP/3
最新版Chrome 83+、Firefox 75+已支持QUIC，但是都需要手动开启，主要原因HTTP/3还在草案阶段
- Firefox——`about:config`中network.http.http3.enabled = true
- Chrome——`chrome://flags`中enable-quic

步骤一：关闭当前Chrome
步骤二：在电脑命令行下启动Chrome
Windows 环境：C:\Program Files (x86)\Google\Chrome\Application>chrome.exe --enable-quic --quic-version=h3-29

macOS 环境 ：/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --enable-quic --quic-version=h3-29



