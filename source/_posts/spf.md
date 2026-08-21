---
title: SPF防范假冒邮件
date: 2021-07-14
categories:
  - SPF
tags:
  - SPF
  - 伪造邮箱
---


如果你购买的域名，没有配置SPF值，则黑客可以冒充你的域名后缀发送邮件。

### 添加TXT记录

```
v=spf1 include:spf.mail.qq.com ip4:1.1.1.1 -all
```

### 检测是否有SPF漏洞
Windows下，命令行下输入：
```bash
nslookup -type=txt 域名
```

Unix下：

```bash
dig 域名 txt +short
```
