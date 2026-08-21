---
title: CF大善人内网穿透
date: 2026-05-13
categories:
  - 内网穿透
tags:
  - Tunnel
  - CloudFlare Mesh
---


## 本地已安装某个应用服务，通过CF暴漏在公网
最终实现效果，通过xxx.heyaff.com来访问VPS上本地某Web服务

### 1、安装 cloudflared（Ubuntu）
```
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
```
### 2、登陆CF授权
`cloudflared tunnel login`<!--more-->
出现一个网址，用浏览器打开登陆CF，选择用哪个域名授权，OK后自动生成/root/.cloudflared/cert.pem和json文件
### 3、创建 Tunnel
`cloudflared tunnel create xxx-tunnel`
输出Tunnel UUID: xxxxxxxx-xxxx

### 4、修改配置文件（后续新增Web站点，直接从这里开始！！！）
```
mkdir -p /etc/cloudflared
vim /etc/cloudflared/config.yml
```

最终文件（注意：前提是本地VPS已经启动了一个Web如31393端口）

```
tunnel: xxxxxxxx-xxxx-xxxx
credentials-file: /root/.cloudflared/xxxxxxxx.json

ingress:
  - hostname: xxx.heyaff.com
    service: http://localhost:31393
  - service: http_status:404
```
### 5、绑定DNS
`cloudflared tunnel route dns xxx-tunnel xxx.heyaff.com`
自动在CF DNS控制台创建CNAME
### 6、启动Tunnel
`cloudflared tunnel run xxx-tunnel`
### 7、设置开机自启
```
cloudflared service install
systemctl enable cloudflared
systemctl start cloudflared
```

## 已安装CF Tunnel，将SSH暴漏在公网

### 1、编辑config.yml添加SSH协议
```
tunnel: UUID
credentials-file: /root/.cloudflared/UUID.json
protocol: http2

ingress:
  - hostname: xxx.heyaff.com
    service: http://localhost:31393

  - hostname: ssh.heyaff.com
    service: ssh://localhost:22

  - service: http_status:404
```

### 2、重启服务
```
systemctl restart cloudflared
systemctl status cloudflared
```

### 3、创建DNS
可以继续复用VPS上已经创建好的tunnel，不用每个Web服务或者SSH协议都单独创建一个tunnel
```
cloudflared tunnel route dns xxx-tunnel ssh.heyaff.com
```
### 4、CF后台开启Web SSH
```
Zero Trust → Networks → Tunnels名字

```
### 5、公网可直接访问ssh.heyaff.com

## 删除CF隧道
如果某个Web站点不想通过CF暴漏在公网，直接第4步骤删除配置中相关域名即可，然后重启CF服务，域名DNS解析可删可不删。



