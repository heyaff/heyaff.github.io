---
title: SSRF科普
date: 2021-12-28
categories:
  - SSRF
tags:
  - SSRF
  - 反弹shell
---


### 目的
通过利用有SSRF漏洞的公网web应用作为代理，攻击内网机房中的其他机器或本地服务器

### 原理
暴漏在公网的有SSRF漏洞的系统，因未对目标地址做过滤与限制，导致执行了恶意的payload，可探测内网其他应用或本机服务文件<!--more-->
![](/images/SSRF.PNG)

### 如何判断是否存在SSRF
在加载图片或url参数时，如外网用户访问www.xxx.com/a.php?image=http://www.abc.com/1.jpg ，这个image参数如果不是本地浏览器发起的请求，那基本是服务端发起的，此时有可能存在SSRF，接下来要验证替换此URL为内网地址看看是否可以回显来判断。

### 利用SSRF做什么
- 扫描内部网络，获取IP、端口信息
- 根据识别出的应用针对性的发送payload攻击，例如struts2
- 向内网`任意主机`的`任意端口`发送精心构造的数据包{Payload}
- 利用file协议读取本地文件，比如file:///etc/passwd

### 绕过域名白名单检测
利用 `@/#` 绕过域名限定的正则：
http://hao.com@evil.com ≈ http://evil.com	（常用）
http://evil.com#hao.com ≈ http://evil.com
http://evil.com?hao.com ≈ http://evil.com
http://evil.com\hao.com ≈ http://evil.com


### SSRF常见绕过技巧
- 利用@符号
- 利用localhost
- 利用短地址	
- 利用特殊域名
- 利用DNS解析
- 利用进制转换
- 其他协议绕过

### payload利用的协议
- file
在有回显的情况下，利用 file 协议可以读取任意内容
- dict
泄露安装软件版本信息，查看端口，操作内网redis服务等
- gopher
gopher支持发出GET、POST请求：可以先截获get请求包和post请求包，再构造成符合gopher协议的请求。gopher协议是ssrf利用中一个最强大的协议(俗称万能协议)。可用于反弹shell

### SSRF进阶
针对SSRF漏洞的系统，透过它去探测内网中的<font color="red">**未授权**</font>Redis，若Redis启动是<font color="red">**root**</font>权限，则可以将反弹shell脚本写入/etc/crontab定时任务URL编码后发送。非Redis的机器，正常情况下，URL请求不会让对方执行反弹shell的

### 反弹shell
-[浅析反弹shell原理](https://www.austfish.cn/2021/04/08/%E6%B5%85%E6%9E%90%E5%8F%8D%E5%BC%B9shell%E5%8E%9F%E7%90%86/ "Fisher's 安全笔记")

### 修复方案

一定要过滤用户端传进来的URL参数
• 限制协议为HTTP、HTTPS，可以防止`file://` `gopher://`等引起的问题
• 如果传进来的是302重定向，要获取最终IP
• 禁止传进来的IP是内网IP，或设置URL白名单

